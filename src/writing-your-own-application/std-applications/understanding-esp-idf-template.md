# Understanding esp-idf-template

Now that we know how to [generate a std project], let's inspect what the generated project contains and try to understand every part of it.

## Inspecting the generated Project

When creating a project from [esp-idf-template] using:

- MCU: `esp32c3`
- ESP-IDF version: `v4.4`
- STD support: `true`
- Devcontainer support: `false`

It should generate a file structure like this:

```text
├── build.rs
├── .cargo
│   └── config.toml
├── Cargo.toml
├── .gitignore
├── rust-toolchain.toml
├── sdkconfig.defaults
└── src
    └── main.rs
```

Before going further let's see what these files are for.

- [.gitignore]
    - tells `git` which folders and files to ignore
- [Cargo.toml]
    - the usual Cargo manifest declaring some meta-data and dependencies of the project
- LICENSE-APACHE, LICENSE_MIT
    - those are the most common licenses used in the Rust ecosystem
    - if you want to apply a different license you can delete these files and change the license in `Cargo.toml`
- [rust-toolchain.toml]
    - defines which Rust toolchain to use
    - depending on your target this will use `nightly` or `esp`
- [.cargo/config.toml]
    - the Cargo configuration
    - contains our target
    - contains `runner = "espflash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
    - contains the linker to use, in our case, [`ldproxy`]
    - contains the unstable `build-std` cargo feature enabled.
    - contains the `ESP-IDF-VERSION` envrionment variable that tells [`esp-idf-sys`] which ESP-IDF version the project will use.
- src/main.rs
    - the main source file of the newly created project
    - we will examine its content in the next section
- [build.rs]
    - propagates linker arguments for `ldproxy`.
- [sdkconfig.defaults]
    - contains the overriden values from the ESP-IDF defaults.

## `main.rs`

```rust,ignore
use esp_idf_sys as _; // If using the `binstart` feature of `esp-idf-sys`, always keep this module imported

fn main() {
    esp_idf_sys::link_patches();
    println!("Hello, world!");
}

```

The first line its an import that defines the esp-idf entry-point when the root crate is a binary crate that defines a main function.

Then, we have an usual main function with two lines on it:

- A call to `esp_idf_sys::link_patches` function that makes sure that a few patches to the ESP-IDF which are implemented in Rust are linked to the final executable.
- We print in our console the famous "Hello World!".

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
MAC address:       60:55:f9:c0:39:7c
App/part. size:    409728/4128768 bytes, 9.92%
[00:00:00] ########################################      12/12      segment 0x0
[00:00:00] ########################################       1/1       segment 0x8000
[00:00:04] ########################################     210/210     segment 0x10000
Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

ESP-ROM:esp32c3-api1-20210207
Build:Feb  7 2021
rst:0x15 (USB_UART_CHIP_RESET),boot:0xc (SPI_FAST_FLASH_BOOT)
Saved PC:0x4004c97e
0x4004c97e - chip726_phyrom_version_num
    at ??:??
SPIWP:0xee
mode:DIO, clock div:1
load:0x3fcd6100,len:0x172c
load:0x403ce000,len:0x928
0x403ce000 - _iram_text_end
    at ??:??
load:0x403d0000,len:0x2ce0
0x403d0000 - _iram_text_end
    at ??:??
entry 0x403ce000
0x403ce000 - _iram_text_end
    at ??:??
I (24) boot: ESP-IDF v4.4-dev-2825-gb63ec47238 2nd stage bootloader
I (24) boot: compile time 12:10:40
I (24) boot: chip revision: 3
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
I (99) esp_image: segment 0: paddr=00010020 vaddr=3c050020 size=17640h ( 95808) map
I (122) esp_image: segment 1: paddr=00027668 vaddr=3fc89c00 size=0146ch (  5228) load
I (123) esp_image: segment 2: paddr=00028adc vaddr=40380000 size=0753ch ( 30012) load
I (133) esp_image: segment 3: paddr=00030020 vaddr=42000020 size=419d8h (268760) map
I (176) esp_image: segment 4: paddr=00071a00 vaddr=4038753c size=02644h (  9796) load
I (178) esp_image: segment 5: paddr=0007404c vaddr=50000010 size=00010h (    16) load
I (185) boot: Loaded app from partition at offset 0x10000
I (188) boot: Disabling RNG early entropy source...
I (205) cpu_start: Pro cpu up.
I (213) cpu_start: Pro cpu start user code
I (213) cpu_start: cpu freq: 160000000
I (213) cpu_start: Application information:
I (216) cpu_start: Project name:     libespidf
I (221) cpu_start: App version:      1
I (226) cpu_start: Compile time:     Nov  3 2022 13:16:23
I (232) cpu_start: ELF file SHA256:  0000000000000000...
I (238) cpu_start: ESP-IDF:          755ce10-dirty
I (243) heap_init: Initializing. RAM available for dynamic allocation:
I (250) heap_init: At 3FC8BF90 len 00050780 (321 KiB): DRAM
I (257) heap_init: At 3FCDC710 len 00002950 (10 KiB): STACK/DRAM
I (263) heap_init: At 50000020 len 00001FE0 (7 KiB): RTCRAM
I (270) spi_flash: detected chip: generic
I (274) spi_flash: flash io: dio
I (279) sleep: Configure to isolate all GPIO pins in sleep state
I (285) sleep: Enable automatic switching of GPIO sleep configuration
I (292) cpu_start: Starting scheduler.
Hello, world!
```

As you can see, there are messages from the first and second stage bootloader and then, our "Hello, world!" its printed.

You can reboot with `CTRL+R` or exit with `CTRL+C`.

[.gitignore]: https://git-scm.com/docs/gitignore
[Cargo.toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[.cargo/config.toml]: https://doc.rust-lang.org/cargo/reference/config.html
[generate a std project]: ../generate-project-from-template.md#esp-idf-template
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[build.rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[sdkconfig.defaults]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/build-system.html#custom-sdkconfig-defaults
[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash
[`runner` configuration]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
