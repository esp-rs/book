# Understanding `esp-template`

Now that we know how to [generate a `no_std` project][generate-no-std], let's inspect what the generated
project contains, try to understand every part of it, and run it.

[generate-no-std]: ./index.md

## Inspecting the Generated Project

When creating a project from [`esp-template`][esp-template] with the following answers:
-  Which MCU to target? · `esp32c3`
- Configure advanced template options? · `false`

For this explanation, we will use the default values, if you want further modifications, see the [additional prompts][prompts] when not using default values.

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

Before going further, let's see what these files are for.

- [`.cargo/config.toml`][config-toml]
    - The Cargo configuration
    - This defines a few options to correctly build the project
    - Contains `runner = "espflash flash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
- `src/main.rs`
    - The main source file of the newly created project
    - For details, see the [Understanding `main.rs`][main-rs] section below
- [`.gitignore`][gitignore]
    - Tells `git` which folders and files to ignore
- [`Cargo.toml`][cargo-toml]
    - The usual Cargo manifest declares some meta-data and dependencies of the project
- `LICENSE-APACHE`, `LICENSE_MIT`
    - Those are the most common licenses used in the Rust ecosystem
    - If you want to use a different license, you can delete these files and change the license in `Cargo.toml`
- [`rust-toolchain.toml`][rust-toolchain-toml]
    - Defines which Rust toolchain to use
      - The toolchain will be `nightly` or `esp` depending on your target

[esp-template]: https://github.com/esp-rs/esp-template
[prompts]: https://github.com/esp-rs/esp-template#esp-template
[main-rs]: #understanding-mainrs
[cargo-toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[gitignore]: https://git-scm.com/docs/gitignore
[config-toml]: https://doc.rust-lang.org/cargo/reference/config.html
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file

### Understanding `main.rs`

```rust,ignore
 1 #![no_std]
 2 #![no_main]
```

- `#![no_std]`
  - This tells the Rust compiler that this code doesn't use `libstd`
- `#![no_main]`
  - The `no_main` attribute says that this program won't use the standard main interface, which is tailored for command-line applications that receive arguments. Instead of the standard main, we'll use the entry attribute from the `riscv-rt` crate to define a custom entry point. In this program, we have named the entry point `main`, but any other name could have been used. The entry point function must be a [diverging function][diverging-function]. I.e. it has the signature `fn foo() -> !`; this type indicates that the function never returns – which means that the program never terminates.

```rust,ignore
 4 use esp_backtrace as _;
 5 use esp_println::println;
 6 use hal::{clock::ClockControl, peripherals::Peripherals, prelude::*, timer::TimerGroup, Rtc};
```
- `use esp_backtrace as _;`
  - Since we are in a bare-metal environment, we need a panic handler that runs if a panic occurs in code
  - There are a few different crates you can use (e.g `panic-halt`) but `esp-backtrace` provides an implementation that prints the address of a backtrace - together with `espflash`/`espmonitor` these addresses can get decoded into source code locations
- `use esp_println::println;`
  - Provides `println!` implementation
- `use hal:{...}`
  - We need to bring in some types we are going to use
  - These are from `esp-hal`

```rust,ignore
 8 #[entry]
 9 fn main() -> ! {
10    let peripherals = Peripherals::take();
11    let mut system = peripherals.SYSTEM.split();
12    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();
13
14    // Disable the RTC and TIMG watchdog timers
15    let mut rtc = Rtc::new(peripherals.RTC_CNTL);
16    let timer_group0 = TimerGroup::new(
17        peripherals.TIMG0,
18        &clocks,
19        &mut system.peripheral_clock_control,
20    );
21    let mut wdt0 = timer_group0.wdt;
22    let timer_group1 = TimerGroup::new(
23        peripherals.TIMG1,
24        &clocks,
25        &mut system.peripheral_clock_control,
26    );
27    rtc.swd.disable();
28    rtc.rwdt.disable();
29    wdt0.disable();
30    wdt1.disable();
31
32    println!("Hello world!");
33
34    loop {}
35 }
```
Inside the `main` function we can find:
- `let peripherals = Peripherals::take().unwrap();`
  - HAL drivers usually take ownership of peripherals accessed via the PAC
  - Here we take all the peripherals from the PAC to pass them to the HAL drivers later
- `let mut system = peripherals.SYSTEM.split();`
  - Sometimes a peripheral (here the System peripheral) is coarse-grained and doesn't exactly fit the HAL drivers - so here we split the System peripheral into smaller pieces which get passed to the drivers
- `let clocks = ClockControl::boot_defaults(system.clock_control).freeze();`
  - Here we configure the system clocks - in this case, we are fine with the defaults
  - We freeze the clocks, which means we can't change them later
  - Some drivers need a reference to the clocks to know how to calculate rates and durations
- The next block of code instantiates some peripherals (namely RTC and the two timer groups) to disable the watchdog, which is armed after boot
  - Without that code, the SoC would reboot after some time
  - There is another way to prevent the reboot: [feeding][wtd-feeding] the watchdog
- `println!("Hello world!");`
  - Prints "Hello world!"
- `loop {}`
  - Since our function is supposed to never return, we just "do nothing" in a loop

[diverging-function]: https://doc.rust-lang.org/beta/rust-by-example/fn/diverging.html
[wtd-feeding]: https://docs.rs/esp32c3-hal/0.10.0/esp32c3_hal/prelude/trait._embedded_hal_watchdog_Watchdog.html#tymethod.feed

## Running the Code

Building and running the code is as easy as

```shell
cargo run
```

This builds the code according to the configuration and executes [`espflash`][espflash] to flash the code to the board.

Since our [`runner` configuration][runner-config] also passes the `--monitor` argument to [`espflash`][espflash], we can see what the code is printing.

Make sure that you have [`espflash`][espflash] installed, otherwise this step will fail. To install [`espflash`][espflash]:
`cargo install espflash`

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
[00:00:01] [========================================]      64/64      0x10000
[2023-04-17T14:17:11Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

...
Hello world!
```

What you see here are messages from the first and second stage bootloader, and then, our "Hello world" message!

And that is exactly what the code is doing.

You can reboot with `CTRL+R` or exit with `CTRL+C`.

If you encounter any issues while building the project, please, see the [Troubleshooting][troubleshooting] chapter.

[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
[runner-config]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[troubleshooting]: ../../troubleshooting/index.md
