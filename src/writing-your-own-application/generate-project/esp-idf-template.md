# Understanding esp-idf-template

Now that we know how to [generate a std project], let's inspect what the generated project contains and try to understand every part of it.

## Inspecting the generated Project

When creating a project from [esp-idf-template] with the following answers:
- Which MCU to target? · `esp32c3`
- Use template default values? · `true`

For this explanation we will use the default values, if you want further modifications, see the [additional prompts] when not using default values.

It should generate a file structure like this:

```text
├── .cargo
│   └── config.toml
└── src
    └── main.rs
├── .gitignore
├── build.rs
├── Cargo.toml
├── rust-toolchain.toml
├── sdkconfig.defaults
```

Before going further, let's see what these files are for.

- [.cargo/config.toml]
    - The Cargo configuration
    - Contains our target
    - Contains `runner = "espflash flash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
    - Contains the linker to use, in our case, [`ldproxy`]
    - Contains the unstable `build-std` cargo feature enabled.
    - Contains the `ESP-IDF-VERSION` environment variable that tells [`esp-idf-sys`] which ESP-IDF version the project will use.
- src/main.rs
    - The main source file of the newly created project
    - For details, see the [`main.rs`] section below.
- [.gitignore]
    - Tells `git` which folders and files to ignore
- [build.rs]
    - Propagates linker arguments for `ldproxy`.
- [Cargo.toml]
    - The usual Cargo manifest declaring some meta-data and dependencies of the project
- [rust-toolchain.toml]
    - Defines which Rust toolchain to use
      - The toolchain will be `nightly` or `esp` depending on your target.
- [sdkconfig.defaults]
    - Contains the overridden values from the ESP-IDF defaults.

## `main.rs`

```rust,ignore
use esp_idf_sys as _; // If using the `binstart` feature of `esp-idf-sys`, always keep this module imported

fn main() {
    // It is necessary to call this function once. Otherwise some patches to the runtime
    // implemented by esp-idf-sys might not link properly. See https://github.com/esp-rs/esp-idf-template/issues/71
    esp_idf_sys::link_patches();
    println!("Hello, world!");
}
```
The first line is an import that defines the esp-idf entry-point when the root crate is a binary crate that defines a main function.

Then, we have a usual main function with a  few lines on it:
- A call to `esp_idf_sys::link_patches` function that makes sure that a few patches to the ESP-IDF which are implemented in Rust are linked to the final executable.
- We print on our console the famous "Hello World!".

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
[00:00:04] [========================================]     227/227     0x10000                                                                   [2023-04-18T08:05:15Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

ESP-ROM:esp32c3-api1-20210207
Build:Feb  7 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0xc (SPI_FAST_FLASH_BOOT)
Saved PC:0x40380816
0x40380816 - esp_restart_noos
    at /home/sergio/Documents/Espressif/tests/esp-rust-app/.embuild/espressif/esp-idf/release-v4.4/components/esp_system/port/soc/esp32c3/system_internal.c:106
SPIWP:0xee
mode:DIO, clock div:2
load:0x3fcd5820,len:0x16ec
0x3fcd5820 - _bss_end
    at ??:??
load:0x403cc710,len:0x95c
0x403cc710 - _iram_data_start
    at ??:??
load:0x403ce710,len:0x2dc0
0x403ce710 - _iram_data_start
    at ??:??
SHA-256 comparison failed:
Calculated: 692c10f3e7d531666ff34d02d3da161b3daa41ea629010d031fe3706fbada122
Expected: 9fafed52ab0387e903bde368d0d6bfffe0dcc3d2f90dca069a4db891108c387c
Attempting to boot anyway...
entry 0x403cc710
0x403cc710 - _iram_data_start
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
I (103) esp_image: segment 0: paddr=00010020 vaddr=3c050020 size=23b88h (146312) map
I (144) esp_image: segment 1: paddr=00033bb0 vaddr=3fc8a000 size=00cd8h (  3288) load
I (145) esp_image: segment 2: paddr=00034890 vaddr=40380000 size=09ea0h ( 40608) load
I (160) esp_image: segment 3: paddr=0003e738 vaddr=00000000 size=018e0h (  6368)
I (161) esp_image: segment 4: paddr=00040020 vaddr=42000020 size=44c88h (281736) map
I (231) boot: Loaded app from partition at offset 0x10000
I (231) boot: Disabling RNG early entropy source...
I (242) cpu_start: Pro cpu up.
I (251) cpu_start: Pro cpu start user code
I (251) cpu_start: cpu freq: 160000000
I (251) cpu_start: Application information:
I (254) cpu_start: Project name:     libespidf
I (259) cpu_start: App version:      1
I (264) cpu_start: Compile time:     Apr 18 2023 10:04:01
I (270) cpu_start: ELF file SHA256:  0000000000000000...
I (276) cpu_start: ESP-IDF:          424ddb3-dirty
I (281) cpu_start: Min chip rev:     v0.3
I (286) cpu_start: Max chip rev:     v0.99
I (291) cpu_start: Chip rev:         v0.3
I (295) heap_init: Initializing. RAM available for dynamic allocation:
I (303) heap_init: At 3FC8BC00 len 00050B10 (322 KiB): DRAM
I (309) heap_init: At 3FCDC710 len 00002950 (10 KiB): STACK/DRAM
I (315) heap_init: At 50000020 len 00001FE0 (7 KiB): RTCRAM
I (323) spi_flash: detected chip: generic
I (327) spi_flash: flash io: dio
I (331) sleep: Configure to isolate all GPIO pins in sleep state
I (337) sleep: Enable automatic switching of GPIO sleep configuration
I (344) cpu_start: Starting scheduler.
Hello, world!
```
As you can see, there are messages from the first and second stage bootloader and then, our "Hello, world!" is printed.

You can reboot with `CTRL+R` or exit with `CTRL+C`.

[additional prompts]: https://github.com/esp-rs/esp-idf-template#generate-the-project
[.gitignore]: https://git-scm.com/docs/gitignore
[Cargo.toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[.cargo/config.toml]: https://doc.rust-lang.org/cargo/reference/config.html
[generate a std project]: ./index.md
[esp-idf-template]: https://github.com/esp-rs/esp-template
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys
[`main.rs`]: #mainrs
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[build.rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[sdkconfig.defaults]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/build-system.html#custom-sdkconfig-defaults
[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash
[`runner` configuration]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
