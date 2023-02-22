# Understanding esp-template

Now that we know how to [generate a no_std project], let's inspect what the generated
project contains and try to understand every part of it.

## Inspecting the generated Project

When creating a project from [esp-template] using:

- MCU: `esp32c3`
- Devcontainer support: `false`
- `esp-alloc` crate support: `flase`

It should generate a file structure like this:

```text
├── .cargo
│   └── config.toml
├── src
│   └── main.rs
├── .vscode
│   └── settings.json
├── .gitignore
├── Cargo.toml
├── LICENSE-APACHE
├── LICENSE-MIT
└── rust-toolchain.toml
```

Before going further let's see what these files are for.

- [.gitignore]
    - tells `git` which folders and files to ignore
- [Cargo.toml]
    - the usual Cargo manifest declaring some meta-data and dependencies of the project
- `LICENSE-APACHE`, `LICENSE-MIT`
    - those are the most common licenses used in the Rust ecosystem
    - if you want to apply a different license you can delete these files and change the license in `Cargo.toml`
- [rust-toolchain.toml]
    - defines which Rust toolchain to use
    - depending on your target this will use `nightly` or `esp`
- [.cargo/config.toml]
    - the Cargo configuration
    - this defines a few options to correctly build the project
    - also contains `runner = "espflash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
- .vscode/settings.json
    - settings for Visual Studio Code - if you are not using VSCode you can delete the whole folder
- src/main.rs
    - the main source file of the newly created project
    - we will examine its content in the next section

## `main.rs`

```rust,ignore
#![no_std]
#![no_main]

use esp32c3_hal::{clock::ClockControl, pac::Peripherals, prelude::*, timer::TimerGroup, Rtc};
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

    loop {}
}
```

That is quite a lot of code. Let's see what it is good for.

- `#![no_std]`
    - this tells the Rust compiler that this code doesn't use `libstd`
- `#![no_main]`
    - The `no_main` attribute says that this program won't use the standard main interface, which is tailored for command-line applications that receive arguments. Instead of the standard main, we'll use the entry attribute from the `riscv-rt` crate to define a custom entry point. In this program we have named the entry point `main`, but any other name could have been used. The entry point function must be a [diverging function]. I.e. it has the signature `fn foo() -> !`; this type indicates that the function never returns – which means that the program never terminates.
- `use esp32c3_hal:{...}`
    - we need to bring in some types we are going to use
    - these are from `esp-hal`
- `use esp_backtrace as _;`
    - since we are in a bare-metal environment we need a panic-handler that runs if a panic occurs in code
    - there are a few different crates you can use (e.g `panic-halt`) but `esp-backtrace` provides an implementation that prints the address of a backtrace - together with `espflash`/`espmonitor` these addresses can get decoded into source code locations
- `let peripherals = Peripherals::take().unwrap();`
    - HAL drivers usually take ownership of peripherals accessed via the PAC
    - here we take all the peripherals from the PAC to pass them to the HAL drivers later
- `let system = peripherals.SYSTEM.split();`
    - sometimes a peripheral (here the System peripheral) is coarse-grained and doesn't exactly fit the HAL drivers - so here we split the System peripheral into smaller pieces which get passed to the drivers
- `let clocks = ClockControl::boot_defaults(system.clock_control).freeze();`
    - here we configure the system clocks - in this case, we are fine with the defaults
    - we freeze the clocks which means we cannot change them later
    - some drivers need a reference to the clocks to know how to calculate rates and durations
- the next block of code instantiates some peripherals (namely RTC and the two timer groups) to disable the watchdog which is armed after boot
    - without that code, the SoC would reboot after some time
    - there is another way to prevent the reboot: [feeding](https://docs.rs/esp32c3-hal/0.2.0/esp32c3_hal/prelude/trait._embedded_hal_watchdog_Watchdog.html#tymethod.feed) the watchdog
- `loop {}`
    - since our function is supposed to never return we just "do nothing" in a loop

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
Connecting...

Chip type:         ESP32-C3 (revision 3)
Crystal frequency: 40MHz
Flash size:        4MB
Features:          WiFi
MAC address:       60:55:f9:c0:0e:ec
App/part. size:    198752/4128768 bytes, 4.81%
[00:00:00] ########################################      12/12      segment 0x0
[00:00:00] ########################################       1/1       segment 0x8000
[00:00:01] ########################################      57/57      segment 0x10000
Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

ESP-ROM:esp32c3-api1-20210207
Build:Feb  7 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0xc (SPI_FAST_FLASH_BOOT)
Saved PC:0x4004c72e
0x4004c72e - _stack_start
    at ??:??
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fcd6100,len:0x172c
load:0x403ce000,len:0x928
0x403ce000 - _erwtext
    at ??:??
load:0x403d0000,len:0x2ce0
0x403d0000 - _erwtext
    at ??:??
entry 0x403ce000
0x403ce000 - _erwtext
    at ??:??
I (24) boot: ESP-IDF v4.4-dev-2825-gb63ec47238 2nd stage bootloader
I (24) boot: compile time 12:10:40
I (25) boot: chip revision: 3
I (28) boot_comm: chip revision: 3, min. bootloader chip revision: 0
I (35) boot.esp32c3: SPI Speed      : 80MHz
I (39) boot.esp32c3: SPI Mode       : DIO
I (44) boot.esp32c3: SPI Flash Size : 4MB
I (49) boot: Enabling RNG early entropy source...
I (54) boot: Partition Table:
I (58) boot: ## Label            Usage          Type ST Offset   Length
I (65) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (73) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (80) boot:  2 factory          factory app      00 00 00010000 003f0000
I (88) boot: End of partition table
I (92) boot_comm: chip revision: 3, min. application chip revision: 0
I (99) esp_image: segment 0: paddr=00010020 vaddr=3c030020 size=04a6ch ( 19052) map
I (110) esp_image: segment 1: paddr=00014a94 vaddr=40380000 size=00910h (  2320) load
I (116) esp_image: segment 2: paddr=000153ac vaddr=00000000 size=0ac6ch ( 44140)
I (131) esp_image: segment 3: paddr=00020020 vaddr=42000020 size=2081ch (133148) map
I (152) boot: Loaded app from partition at offset 0x10000

```

What you see here are messages from the first and second stage bootloader and then ... nothing.

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
