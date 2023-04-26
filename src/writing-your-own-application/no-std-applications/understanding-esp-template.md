# Understanding esp-template

Now that we know how to [generate a no_std project], let's inspect what the generated
project contains and try to understand every part of it.

## Inspecting the generated Project

When creating a project from [esp-template] with the following answers:
-  Which MCU to target? · esp32c3
-  Use template default values? · true

It should generate a file structure like this:

```text
├── .cargo
│   └── config.toml
├── src
│   └── main.rs
├── .gitignore
├── Cargo.toml
├── LICENSE-APACHE
├── LICENSE-MIT
└── rust-toolchain.toml
```

Before going further let's see what these files are for.

- [.cargo/config.toml]
    - The Cargo configuration
    - This defines a few options to correctly build the project
    - Contains `runner = "espflash flash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
- src/main.rs
    - The main source file of the newly created project
    - We will examine its content in the next section
- [.gitignore]
    - Tells `git` which folders and files to ignore
- [Cargo.toml]
    - The usual Cargo manifest declaring some meta-data and dependencies of the project
- LICENSE-APACHE, LICENSE_MIT
    - Those are the most common licenses used in the Rust ecosystem
    - If you want to apply a different license you can delete these files and change the license in `Cargo.toml`
- [rust-toolchain.toml]
    - Defines which Rust toolchain to use
    - Depending on your target this will use `nightly` or `esp`

## `main.rs`

```rust,ignore
#![no_std]
#![no_main]

use esp_backtrace as _;
use esp_println::println;
use hal::{clock::ClockControl, peripherals::Peripherals, prelude::*, timer::TimerGroup, Rtc};

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

    loop {}
}
```

That is quite a lot of code. Let's see what it is good for.

- `#![no_std]`
  - This tells the Rust compiler that this code doesn't use `libstd`
- `#![no_main]`
  - The `no_main` attribute says that this program won't use the standard main interface, which is tailored for command-line applications that receive arguments. Instead of the standard main, we'll use the entry attribute from the `riscv-rt` crate to define a custom entry point. In this program we have named the entry point `main`, but any other name could have been used. The entry point function must be a [diverging function]. I.e. it has the signature `fn foo() -> !`; this type indicates that the function never returns – which means that the program never terminates.
- `use esp_backtrace as _;`
  - Since we are in a bare-metal environment we need a panic-handler that runs if a panic occurs in code
  - There are a few different crates you can use (e.g `panic-halt`) but `esp-backtrace` provides an implementation that prints the address of a backtrace - together with `espflash`/`espmonitor` these addresses can get decoded into source code locations
- `use esp_println::println;`
  - Provices `println!` implementation
- `use hal:{...}`
  - We need to bring in some types we are going to use
  - These are from `esp-hal`
- `let peripherals = Peripherals::take().unwrap();`
  - HAL drivers usually take ownership of peripherals accessed via the PAC
  - Here we take all the peripherals from the PAC to pass them to the HAL drivers later
- `let system = peripherals.SYSTEM.split();`
  - Sometimes a peripheral (here the System peripheral) is coarse-grained and doesn't exactly fit the HAL drivers - so here we split the System peripheral into smaller pieces which get passed to the drivers
- `let clocks = ClockControl::boot_defaults(system.clock_control).freeze();`
  - Here we configure the system clocks - in this case, we are fine with the defaults
  - We freeze the clocks which means we cannot change them later
  - Some drivers need a reference to the clocks to know how to calculate rates and durations
- The next block of code instantiates some peripherals (namely RTC and the two timer groups) to disable the watchdog which is armed after boot
  - Without that code, the SoC would reboot after some time
  - There is another way to prevent the reboot: [feeding](https://docs.rs/esp32c3-hal/0.2.0/esp32c3_hal/prelude/trait._embedded_hal_watchdog_Watchdog.html#tymethod.feed) the watchdog
- `println!("Hello world!");`
  - Prints "Hello Wolrd!"
- `loop {}`
  - Since our function is supposed to never return we just "do nothing" in a loop

## Running the Code

Building and running the code is as easy as

```shell
cargo run
```

This builds the code according to the configuration and executes [`espflash`] to flash the code to the board.

Since our [`runner` configuration] also passes the `--monitor` argument to [`espflash`] we can see what the code is printing.

> Make sure that you have [`espflash`] installed, otherwise this step will fail. To install [`espflash`]:
> `cargo install espflash`

You should see something similar to this:
```text
[2023-04-17T14:17:08Z INFO ] Serial port: '/dev/ttyACM0'
[2023-04-17T14:17:08Z INFO ] Connecting...
[2023-04-17T14:17:09Z INFO ] Using flash stub
[2023-04-17T14:17:09Z WARN ] Setting baud rate higher than 115,200 can cause issues
Chip type:         esp32c3 (revision v0.3)
Crystal frequency: 40MHz
Flash size:        4MB
Features:          WiFi, BLE
MAC address:       60:55:f9:c0:39:7c
App/part. size:    203,920/4,128,768 bytes, 4.94%
[00:00:00] [========================================]      13/13      0x0
[00:00:00] [========================================]       1/1       0x8000
[00:00:01] [========================================]      64/64      0x10000                                                                                                                    [2023-04-17T14:17:11Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

ESP-ROM:esp32c3-api1-20210207
Build:Feb  7 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0xc (SPI_FAST_FLASH_BOOT)
Saved PC:0x40380816
0x40380816 -
    at ??:??
SPIWP:0xee
mode:DIO, clock div:2
load:0x3fcd5820,len:0x16ec
0x3fcd5820 - _stack_start
    at ??:??
load:0x403cc710,len:0x95c
0x403cc710 -
    at ??:??
load:0x403ce710,len:0x2dc0
0x403ce710 -
    at ??:??
SHA-256 comparison failed:
Calculated: 692c10f3e7d531666ff34d02d3da161b3daa41ea629010d031fe3706fbada122
Expected: 9fafed52ab0387e903bde368d0d6bfffe0dcc3d2f90dca069a4db891108c387c
Attempting to boot anyway...
entry 0x403cc710
0x403cc710 -
    at ??:??
I (43) boot: ESP-IDF v5.0-beta1-764-gdbcf640261 2nd stage bootloader
I (43) boot: compile time 11:30:26
I (43) boot: chip revision: V003
I (46) boot.esp32c3: SPI Speed      : 40MHz
I (51) boot.esp32c3: SPI Mode       : DIO
I (56) boot.esp32c3: SPI Flash Size : 4MB
I (61) boot: Enabling RNG early entropy source...
I (66) boot: Partition Table:
I (70) boot: ## Label            Usage          Type ST Offset   Length
I (77) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (84) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (92) boot:  2 factory          factory app      00 00 00010000 003f0000
I (99) boot: End of partition table
I (103) esp_image: segment 0: paddr=00010020 vaddr=3c030020 size=05f74h ( 24436) map
I (117) esp_image: segment 1: paddr=00015f9c vaddr=40380000 size=007f8h (  2040) load
I (121) esp_image: segment 2: paddr=0001679c vaddr=00000000 size=0987ch ( 39036)
I (137) esp_image: segment 3: paddr=00020020 vaddr=42000020 size=21c40h (138304) map
I (168) boot: Loaded app from partition at offset 0x10000
I (168) boot: Disabling RNG early entropy source...
Hello world!
```

What you see here are messages from the first and second stage bootloader and then ... our "Hello World" message!

And that is exactly what the code is doing.

You can reboot with `CTRL+R` or exit with `CTRL+C`.

In the next chapter, we will add some more interesting output.

[generate a no_std project]: ../generate-project-from-template.md#esp-template
[esp-template]: https://github.com/esp-rs/esp-template
[.gitignore]: https://git-scm.com/docs/gitignore
[Cargo.toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[.cargo/config.toml]: https://doc.rust-lang.org/cargo/reference/config.html
[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash
[`runner` configuration]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[diverging function]: https://doc.rust-lang.org/beta/rust-by-example/fn/diverging.html
