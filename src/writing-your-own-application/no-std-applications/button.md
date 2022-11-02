# Detect a button press

The dev-boards have a button (labeled BOOT) on GPIO9. Let's see how to check the state of the button.

```rust,ignore
#![no_std]
#![no_main]

use esp32c3_hal::{
    clock::ClockControl, pac::Peripherals, prelude::*, timer::TimerGroup, Rtc, IO,
};
use esp_backtrace as _;

#[riscv_rt::entry]
fn main() -> ! {
    let peripherals = Peripherals::take().unwrap();
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

    // Set GPIO7 as an output, GPIO9 as input
    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut led = io.pins.gpio7.into_push_pull_output();
    let button = io.pins.gpio9.into_pull_up_input();

    loop {
        if button.is_high().unwrap() {
            led.set_high().unwrap();
        } else {
            led.set_low().unwrap();
        }
    }
}
```

Now if the button is not pressed the LED is lit. If the button is pressed the LED is off.

Similarly to turning a GPIO into an output we can turn it into an input. Then we can get the current state of the input pin with `is_high` and similar functions.
