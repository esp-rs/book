# Using the Rust Standard Library (`std`)

Espressif provides a C-based development framework called [esp-idf] which has support for all Espressif chips starting with the ESP32; note that this framework does _not_ support the ESP8266.

`esp-idf` in turn provides a [newlib] environment with enough functionality to build the Rust standard library (`std`) on top of it. This is the approach that is being taken to enable `std` support on ESP devices.

## Chip Support

In order for applications targeting `std` to be built for ESP devices, two things are required:

1. LLVM/Clang support
2. Support for the device in question in `esp-idf`

Refer to the table below to see if your chip is supported.

|   Chip   | Supported? |
| :------: | :--------: |
|  ESP32   |     ✅     |
| ESP32-C3 |     ✅     |
| ESP32-S2 |     ✅     |
| ESP32-S3 |     ✅     |
| ESP32-H2 |  planned   |
| ESP8266  |     ❌     |

## Standard Library Features

The supported `std` features are as follows:

- Threads
- Mutexes and other synchronization primitives
- Collections
- Random number generation
- Sockets

In addition to the `std` features there is an [embedded-svc] implementation for `esp-idf`, [esp-idf-svc], which adds extra support for services/modules not available in the standard library, including:

- Wi-Fi management
- NVS (non-volatile storage)
- Networking services like `httpd` and `ping`

In general, this approach should feel quite similar to developing for most normal PC environments.

[esp-idf]: https://github.com/espressif/esp-idf
[newlib]: https://sourceware.org/newlib/
[embedded-svc]: https://github.com/esp-rs/embedded-svc
[esp-idf-svc]: https://github.com/esp-rs/esp-idf-svc

## Relevant `esp-rs` crates

| Repository            | Description                                                                                                    |
| --------------------- | -------------------------------------------------------------------------------------------------------------- |
| [esp-rs/esp-idf-hal]  | An implementation of the `embedded-hal` traits using the `esp-idf` framework. Provides the Rust `std` library. |
| [esp-rs/embedded-svc] | Abstraction traits for embedded services.                                                                      |
| [esp-rs/esp-idf-svc]  | An implementation of [embedded-svc] using `esp-idf` drivers.                                                   |
| [esp-rs/esp-idf-sys]  | Rust bindings to the `esp-idf` development framework. Gives raw (`unsafe`) access to drivers, Wi-Fi and more.  |

[esp-rs/embedded-svc]: https://github.com/esp-rs/embedded-svc
[esp-rs/esp-idf-svc]: https://github.com/esp-rs/esp-idf-svc
[esp-rs/esp-idf-sys]: https://github.com/esp-rs/esp-idf-sys
[esp-rs/esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal
