# Understanding `esp-idf-template`

Now that we know how to [generate a `std` project][generate-std], let's inspect what the generated project contains and try to understand every part of it.

[generate-std]: ./index.md

## Inspecting the Generated Project

When creating a project from [`esp-idf-template`][esp-idf-template] with the following answers:
- Which MCU to target? · `esp32c3`
- Configure advanced template options? · `false`

For this explanation, we will use the default values, if you want further modifications, see the [additional prompts][prompts] when not using default values.

It should generate a file structure like this:

```text
├── .cargo
│   └── config.toml
├── src
│   └── main.rs
├── .gitignore
├── build.rs
├── Cargo.toml
├── rust-toolchain.toml
└── sdkconfig.defaults
```

Before going further, let's see what these files are for.

- [`.cargo/config.toml`][config-toml]
    - The Cargo configuration
    - Contains our target
    - Contains `runner = "espflash flash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
    - Contains the linker to use, in our case, [`ldproxy`][ldproxy]
    - Contains the unstable `build-std` Cargo feature enabled
    - Contains the `ESP-IDF-VERSION` environment variable that tells [`esp-idf-sys`][esp-idf-sys] which ESP-IDF version the project will use
- `src/main.rs`
    - The main source file of the newly created project
    - For details, see the [Understanding `main.rs`][main-rs] section below
- [`.gitignore`][gitignore]
    - Tells `git` which folders and files to ignore
- [`build.rs`][build-rs]
    - Propagates linker arguments for `ldproxy`
- [`Cargo.toml`][cargo-toml]
    - The usual Cargo manifest declaring some meta-data and dependencies of the project
- [`rust-toolchain.toml`][rust-toolchain-toml]
    - Defines which Rust toolchain to use
      - The toolchain will be `nightly` or `esp` depending on your target
- [`sdkconfig.defaults`][sdkconfig-defaults]
    - Contains the overridden values from the ESP-IDF defaults

[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[prompts]: https://github.com/esp-rs/esp-idf-template#generate-the-project
[main-rs]:#understanding-mainrs
[config-toml]: https://doc.rust-lang.org/cargo/reference/config.html
[ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys]: https://github.com/esp-rs/esp-idf-sys
[gitignore]: https://git-scm.com/docs/gitignore
[build-rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[cargo-toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[sdkconfig-defaults]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/build-system.html#custom-sdkconfig-defaults

### Understanding `main.rs`

```rust,ignore
1 use esp_idf_sys as _; // If using the `binstart` feature of `esp-idf-sys`, always keep this module imported
2
3 fn main() {
4     // It is necessary to call this function once. Otherwise some patches to the runtime
5     // implemented by esp-idf-sys might not link properly. See https://github.com/esp-rs/esp-idf-template/issues/71
6     esp_idf_sys::link_patches();
7     println!("Hello, world!");
8 }
```

The first line is an import that defines the ESP-IDF entry point when the root crate is a binary crate that defines a main function.

Then, we have a usual main function with a  few lines on it:
- A call to `esp_idf_sys::link_patches` function that makes sure that a few patches to the ESP-IDF which are implemented in Rust are linked to the final executable
- We print on our console the famous "Hello, world!"

## Running the Code

Building and running the code is as easy as

```shell
cargo run
```

This builds the code according to the configuration and executes [`espflash`][espflash]  to flash the code to the board.

Since our [`runner` configuration][runner-config] also passes the `--monitor` argument to [`espflash`][espflash], we can see what the code is printing.

Make sure that you have [`espflash`][espflash] installed, otherwise this step will fail. To install [`espflash`][espflash]:
`cargo install espflash`

You should see something similar to this:

```text
[2023-04-18T08:05:09Z INFO ] Connecting...
[2023-04-18T08:05:10Z INFO ] Using flash stub
[2023-04-18T08:05:10Z WARN ] Setting baud rate higher than 115,200 can cause issues
Chip type:         esp32c3 (revision v0.3)
Crystal frequency: 40MHz
Flash size:        4MB
Features:          WiFi, BLE
MAC address:       60:55:f9:c0:39:7c
App/part. size:    478,416/4,128,768 bytes, 11.59%
[00:00:00] [========================================]      13/13      0x0
[00:00:00] [========================================]       1/1       0x8000
[00:00:04] [========================================]     227/227     0x10000
[2023-04-18T08:05:15Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

...
I (344) cpu_start: Starting scheduler.
Hello, world!
```

As you can see, there are messages from the first and second-stage bootloader and then, our "Hello, world!" is printed.

You can reboot with `CTRL+R` or exit with `CTRL+C`.

If you encounter any issues while building the project, please, see the [Troubleshooting][troubleshooting] chapter.

[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
[runner-config]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[troubleshooting]: ../../troubleshooting/index.md
