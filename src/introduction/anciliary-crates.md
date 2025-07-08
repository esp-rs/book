# Anciliary Crates Overview

The `esp-hal` repository is home to multiple crates that are used to interact with the hardware of the Espressif devices:

| Crate                    | Description                                                                                            | Stability |
| ------------------------ | ------------------------------------------------------------------------------------------------------ | --------- |
| `esp-alloc`              | Memory allocation utilities.                                                                           | Unstable  |
| `esp-backtrace`          | Provides backtraces and support for exceptions and panic handlers .                                    | Unstable  |
| `esp-bootloader-esp-idf` | Offers additional support for the ESP-IDF 2nd stage bootloader.                                        | Unstable  |
| `esp-build`              | Build utilities for use with esp-hal and other related packages, intended for use in build scripts.    | Unstable  |
| `esp-config`             | Configuration system.                                                                                  | Unstable  |
| `esp-hal`                | Bare-metal (`no_std`) HAL for all Espressif ESP32 devices.                                             | Stable *  |
| `esp-hal-embassy`        | Embassy support for `esp-hal`.                                                                         | Unstable  |
| `esp-hal-proc-macros`    | Procedural macros for use with the esp-hal family of HAL packages.                                     | Unstable  |
| `esp-ieee802154`         | Low-level IEEE 802.15.4 protocol support.                                                              | Unstable  |
| `esp-lp-hal`             | Bare-metal (`no_std`) HAL for the low power and ultra-low power cores found in some Espressif devices. | Unstable  |
| `esp-metadata`           | Metadata for Espressif devices, primarily intended for use in build scripts.                           | Unstable  |
| `esp-println`            | Print and logging functionality for Espressif devices.                                                 | Unstable  |
| `esp-riscv-rt`           | Minimal startup/runtime for RISC-V CPUs from Espressif.                                                | Unstable  |
| `esp-storage`            | Storage utilities for Espressif devices.                                                               | Unstable  |
| `esp-wifi`               | Wi-Fi, BLE and ESP-NOW functionality for Espressif devices.                                            | Unstable  |
| `xtensa-lx`              | Low-level access to Xtensa LX processors and peripherals.                                              | Unstable  |
| `xtensa-lx-rt`           | Minimal startup/runtime for Xtensa LX CPUs from Espressif.                                             | Unstable  |

> ⚠️ **Note**: Only GPIOs, UART, SPI, I2C peripherals are stable in the `esp-hal` crate. See [tracking issue][tracking-issue] for more details.

[tracking-issue]: https://github.com/esp-rs/esp-hal/issues/2491
