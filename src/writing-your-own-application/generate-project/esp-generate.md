# Understanding `esp-generate`

Now that we know how to [generate a `no_std` project][generate-no-std], let's inspect what the generated
project contains, try to understand every part of it, and run it.

[generate-no-std]: ./index.md

## Inspecting the Generated Project

When creating a project from [`esp-generate`][esp-generate] with no extra options:
```
esp-generate --chip esp32c3 your-project
```

It should generate a file structure like this:

```text
├── build.rs
├── .cargo
│   └── config.toml
├── Cargo.toml
├── .gitignore
├── rust-toolchain.toml
├── src
│   ├── bin
│   │   └── main.rs
│   └── lib.rs
└── .vscode
    └── settings.json
```

Before going further, let's see what these files are for.
- [`build.rs`][build.rs]
    - Sets the linker script arguments based on the template options.
- [`.cargo/config.toml`][config-toml]
    - The Cargo configuration
    - This defines a few options to correctly build the project
    - Contains the custom runner command for `espflash` or `probe-rs`. For example, `runner = "espflash flash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
- [`Cargo.toml`][cargo-toml]
    - The usual Cargo manifest declares some meta-data and dependencies of the project
- [`.gitignore`][gitignore]
    - Tells `git` which folders and files to ignore
- [`rust-toolchain.toml`][rust-toolchain-toml]
    - Defines which Rust toolchain to use
      - The toolchain will be `nightly` or `esp` depending on your target
- `src/bin/main.rs`
    - The main source file of the newly created project
    - For details, see the [Understanding `main.rs`][main-rs] section below
- `src/lib.rs`
    - This tells the Rust compiler that this code doesn't use `libstd`
- `.vscode/settings.json`
    - Defines a set of settings for Visual Studio Code to make Rust Analyzer work.

[esp-generate]: https://github.com/esp-rs/esp-generate
[build.rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
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
  - The `no_main` attribute says that this program won't use the standard main interface, which is usually used when a full operating system is available. Instead of the standard main, we'll use the entry attribute from the `esp-riscv-rt` crate to define a custom entry point. In this program, we have named the entry point `main`, but any other name could have been used. The entry point function must be a [diverging function][diverging-function]. I.e. it has the signature `fn foo() -> !`; this type indicates that the function never returns – which means that the program never terminates.

```rust,ignore
4 use esp_backtrace as _;
5 use esp_hal::delay::Delay;
6 use esp_hal::prelude::*;
7 use log::info;
```
- `use esp_backtrace as _;`
  - Since we are in a bare-metal environment, we need a panic handler that runs if a panic occurs in code
  - There are a few different crates you can use (e.g `panic-halt`) but `esp-backtrace` provides an implementation that prints the address of a backtrace - together with `espflash` these addresses can get decoded into source code locations
- `use esp_hal::delay::Delay;`
  - Provides `Delay` driver implementation.
- `use esp_hal::prelude::*;`
  - Imports the `esp-hal` [prelude][prelude].

```rust,ignore
 8 #[entry]
 9 fn main() -> ! {
10    esp_println::logger::init_logger_from_env();
11
12    let delay = Delay::new();
13    loop {
14      info!("Hello world!");
15      delay.delay(500.millis());
16    }
17 }
```

Inside the `main` function we can find:
- `esp_println::logger::init_logger_from_env();`
  - Initializes the logger, if `ESP_LOG` environment variable is defined, it will use that log level.
- `let delay = Delay::new();`
  - Creates a delay instance.
- `loop {}`
  - Since our function is supposed to never return, we use a loop
- `info!("Hello world!");`
  - Creates a log message with `info` level that prints "Hello world!".
- `delay.delay(500.millis());`
  - Waits for 500 milliseconds.

[diverging-function]: https://doc.rust-lang.org/beta/rust-by-example/fn/diverging.html

## Running the Code

Building and running the code is as easy as

```shell
cargo run --release
```

This builds the code according to the configuration and executes [`espflash`][espflash] to flash the code to the board.

Since our [`runner` configuration][runner-config] also passes the `--monitor` argument to [`espflash`][espflash], we can see what the code is printing.

Make sure that you have [`espflash`][espflash] installed, otherwise this step will fail. To install [`espflash`][espflash]:
`cargo install espflash`

You should see something similar to this:

```text
...
[2024-11-14T09:29:32Z INFO ] Serial port: '/dev/ttyUSB0'
[2024-11-14T09:29:32Z INFO ] Connecting...
[2024-11-14T09:29:32Z INFO ] Using flash stub
[2024-11-14T09:29:33Z WARN ] Setting baud rate higher than 115,200 can cause issues
Chip type:         esp32c3 (revision v0.3)
Crystal frequency: 40 MHz
Flash size:        4MB
Features:          WiFi, BLE
MAC address:       a0:76:4e:5a:d2:c8
App/part. size:    76,064/4,128,768 bytes, 1.84%
[00:00:00] [========================================]      13/13      0x0
[00:00:00] [========================================]       1/1       0x8000
[00:00:00] [========================================]      11/11      0x10000
[2024-11-14T09:29:35Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit
...
INFO - Hello world!
```

What you see here are messages from the first and second stage bootloader, and then, our "Hello world" message!

And that is exactly what the code is doing.

You can reboot with `CTRL+R` or exit with `CTRL+C`.

If you encounter any issues while building the project, please, see the [Troubleshooting][troubleshooting] chapter.


[prelude]: https://doc.rust-lang.org/reference/names/preludes.html
[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
[runner-config]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[troubleshooting]: ../../troubleshooting/index.md
