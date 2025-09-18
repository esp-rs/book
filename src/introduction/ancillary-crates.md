# Ancillary Crates Overview

Before going into a detailed description of the various packages and concepts for writing an application, it might be useful to have a superficial familiarity with the whole ecosystem, both in the `esp-hal` sense and in the broader Embedded Rust sense.

## `esp-hal` Ecosystem 

The first step in working with a project is to create it, the main way to do this is to use `esp-generate`. Read more about it in the [section](../getting-started/tooling/esp-generate.md) dedicated to this tool.
 
The core crate that ties all work with Espressif chips in Rust is the `esp-hal` crate. Through it, you will be able to perform basic initialization of the chip, as well as access drivers for the peripherals available on the chip. The full [`esp-hal` documentation] for selected chip will give unambiguous information about what peripherals are available to use, and the stability of their respective drivers.

Furthermore, you may want to use more advanced functionality of the chip. For example, network and connectivity. This part of the ecosystem is the responsibility of `esp-radio`, which combines drivers for the communication protocols available on one or another of Espressif's products: `Wi-Fi`, `BLE`, `esp-now` and low-level `IEEE 802.15.4` for the lower layers of communication. More detailed information for each chip is available in the [`esp-radio` sub-repository].

For more advanced work with chip memory and to use collections from the `alloc` crate in `no_std` that require heap allocation, you are welcome to use `esp-alloc`. A separate [chapter in the book](./../application-development/alloc.md) is devoted to this.

The table below briefly describes all crates in the `esp-hal` ecosystem and their purposes:

| Crate                    | Description                                                                                                    |       Stability       |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- | --------------------- |
| `esp-alloc`              | Memory allocation utilities.                                                                                   | Unstable              |
| `esp-backtrace`          | Provides backtraces support.                                                                                   | Unstable              |
| `esp-bootloader-esp-idf` | Offers additional support for the ESP-IDF 2nd stage bootloader, including OTA.                                 | Unstable              |
| `esp-build`              | Build utilities for use with esp-hal and other related packages, intended for use in build scripts.            | Unstable              |
| `esp-config`             | Configuration system.                                                                                          | Unstable              |
| `esp-hal`                | Bare-metal (`no_std`) HAL for all Espressif ESP32 devices.                                                     | Stable<sup>*</sup>    |
| `esp-hal-embassy`        | Embassy support for `esp-hal`.                                                                                 | Unstable              |
| `esp-hal-proc-macros`    | Procedural macros for use with the esp-hal family of HAL packages.                                             | Unstable              |
| `esp-lp-hal`             | Bare-metal (`no_std`) HAL for the low power and ultra-low power cores found in some Espressif devices.         | Unstable              |
| `esp-metadata`           | Metadata for Espressif devices, primarily intended for use in build scripts.                                   | Unstable              |
| `esp-preempt`            | Threading and thread-aware synchronization primitives, primarily used in `esp-radio` when not using an RTOS.   | Unstable              |
| `esp-println`            | Print and logging functionality for Espressif devices.                                                         | Unstable              |
| `esp-riscv-rt`           | Minimal startup/runtime for RISC-V CPUs from Espressif.                                                        | Unstable              |
| `esp-rom-sys`            | ROM code support.                                                                                              | Unstable              |
| `esp-sync`               | Synchronization primitives for Espressif devices.                                                              | Unstable              |
| `esp-storage`            | Storage utilities for Espressif devices.                                                                       | Unstable              |
| `esp-radio`              | Wi-Fi, BLE, IEEE8 802.15.4 and ESP-NOW functionality for Espressif devices.                                    | Unstable              |
| `xtensa-lx`              | Low-level access to Xtensa LX processors and peripherals.                                                      | Unstable              |
| `xtensa-lx-rt`           | Minimal startup/runtime for Xtensa LX CPUs from Espressif.                                                     | Unstable              |

> ⚠️ **Note**: The stability of individual peripheral drivers within `esp-hal` varies. Refer to the [Peripheral support section][peripheral-support] for per-peripheral stability details.

[peripheral-support]: https://github.com/esp-rs/esp-hal/tree/main/esp-hal#peripheral-support

## Embedded Rust Ecosystem Integration

The most popular Hardware Abstraction Layer in the Embedded Rust environment is [`embedded-hal`], which provides a number of traits for several peripherals that allow writing HAL agnostic device drivers. `esp-hal` implements these traits within its drivers. In addition, various traits from different crates used in the embedded Rust industry have also been implemented, such as [`rand_core`] traits for our RNG peripheral, as well as [`embedded-io`] traits, which are analogues of `std::io` traits for `no_std` applications.

[`esp-hal` documentation]: https://docs.espressif.com/projects/rust/esp-hal/latest/
[`esp-radio` sub-repository]: https://github.com/esp-rs/esp-hal/tree/main/esp-radio
[`embedded-hal`]: https://docs.rs/embedded-hal/latest/embedded_hal/index.html
[`rand_core`]: https://crates.io/crates/rand_core
[`embedded-io`]: https://crates.io/crates/embedded-io