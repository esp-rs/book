# Detect a button press with interrupt
[Interrupts] offer a mechanism by which the processor handles asynchronous events and fatal errors.

Let's add the [`critical-section`] crate [(see instructions on how to add a dependency)], and change `main.rs` to look like this:
```rust,ignore
#![no_std]
#![no_main]

use core::cell::RefCell;
use critical_section::Mutex;
use esp_backtrace as _;
use esp_println::println;
use hal::{
    clock::ClockControl,
    gpio::{Event, Gpio9, Input, PullDown, IO},
    interrupt,
    peripherals::{self, Peripherals},
    prelude::*,
    riscv,
    timer::TimerGroup,
    Delay, Rtc,
};

static BUTTON: Mutex<RefCell<Option<Gpio9<Input<PullDown>>>>> = Mutex::new(RefCell::new(None));

#[entry]
fn main() -> ! {
    let peripherals = Peripherals::take();
    let system = peripherals.SYSTEM.split();
    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();

    // Disable the RTC and TIMG watchdog timers
    let mut rtc = Rtc::new(peripherals.RTC_CNTL);
    let timer_group0 = TimerGroup::new(peripherals.TIMG0, &clocks);
    let mut wdt0 = timer_group0.wdt;
    let timer_group1 = TimerGroup::new(peripherals.TIMG1, &clocks);
    let mut wdt1 = timer_group1.wdt;

    rtc.swd.disable();
    rtc.rwdt.disable();
    wdt0.disable();
    wdt1.disable();

    println!("Hello world!");

    // Set GPIO7 as an output, and set its state high initially.
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut led = io.pins.gpio7.into_push_pull_output();

    // Set GPIO9 as an input
    let mut button = io.pins.gpio9.into_pull_down_input();
    button.listen(Event::FallingEdge);

    critical_section::with(|cs| BUTTON.borrow_ref_mut(cs).replace(button));

    interrupt::enable(peripherals::Interrupt::GPIO, interrupt::Priority::Priority3).unwrap();

    unsafe {
        riscv::interrupt::enable();
    }

    let mut delay = Delay::new(&clocks);
    loop {
        led.toggle().unwrap();
        delay.delay_ms(500u32);
    }
}

#[interrupt]
fn GPIO() {
    critical_section::with(|cs| {
        println!("GPIO interrupt");
        BUTTON
            .borrow_ref_mut(cs)
            .as_mut()
            .unwrap()
            .clear_interrupt();
    });
}
```

There are quite a lot of new things here.

First thing is the `static BUTTON`. We need it since in the interrupt handler we have to clear the pending interrupt on the button and we somehow need to pass the button from main to the interrupt handler.

Since an interrupt handler can't have arguments we need a static to get the button into the interrupt handler.

We need the `Mutex` to make access to the button safe.

> Please note that this is not the Mutex you might know from `libstd` but it's the Mutex from [`critical-section`] (and that's why we need to add it as a dependency).

Then we need to call `listen` on the `output` pin to configure the peripheral to raise interrupts. We can raise interrupts for different events - here we want to raise the interrupt on the falling edge.

In the next line we move our button into the `static BUTTON` for the interrupt handler to get hold of it.

Last thing we need to do is actually enable the interrupt.

First parameter here is the kind of interrupt we want. There are several [possible interrupts].

Second parameter is the priority of the interrupt.

The interrupt handler is defined via the `#[interrupt]` macro.
Here the name of the function must match the interrupt.


[Interrupts]: https://docs.rust-embedded.org/book/start/interrupts.html
[`critical-section`]: https://crates.io/crates/critical-section
[(see instructions on how to add a dependency)]: https://doc.rust-lang.org/cargo/guide/dependencies.html
[possible interrupts]: https://docs.rs/esp32c3/0.5.1/esp32c3/enum.Interrupt.html
