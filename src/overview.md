# Ecosystem Overview

As it stands there are two approaches for using Rust on Espressif chips, `std` and `no_std`. Both approaches have their advantages and disadvantages, so you should choose one based on your project's needs. Below is a brief overview of the two approaches.

The [esp-rs organization] on GitHub is home to a number of repositories related to running Rust on ESP. Most of the required crates have their source code hosted here.

[esp-rs organization]: https://github.com/esp-rs/

## Using the Rust Standard Library (`std`)

Espressif provides a C-based development framework called [`esp-idf`] which has support for Espressif chips from the ESP32 and later. `esp-idf` provides a [newlib] environment with enough functionality to build the Rust standard library (`std`) on top of it, hence this approach. The supported `std` features are as follows:

- Threads
- Mutexes and other synchronization primitives
- Collections
- Random number generation
- Sockets

On top of `std` features, there is a [embedded-svc] implementation for `esp-idf` which adds extra support for services/modules not available in the standard library, including:

- Wi-Fi management
- NVS (non-volatile storage)
- Networking services like `httpd` and `ping`

In general, this approach should feel quite similar to developing for most normal PC environments.

[`esp-idf`]: https://github.com/espressif/esp-idf
[newlib]: https://sourceware.org/newlib/
[embedded-svc]: https://github.com/esp-rs/embedded-svc

### Chip Support

Chip support for `std` requires two things, LLVM/Clang support and support in `esp-idf`. Refer to the table below to see if your chip is supported.

|   Chip   | Supported? |
| :------: | :--------: |
|  ESP32   |     ✅     |
| ESP32-C3 |     ✅     |
| ESP32-S2 |     ✅     |
| ESP32-S3 |     ✅     |
| ESP32-H2 |  planned   |
| ESP8266  |     ❌     |

### Relevant `esp-rs` crates

| Repository            | Description                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| [esp-rs/embedded-svc] | Abstraction traits for embedded services.                                                                     |
| [esp-rs/esp-idf-svc]  | An implementation of [embedded-svc] using `esp-idf` drivers.                                                  |
| [esp-rs/esp-idf-sys]  | Rust bindings to the `esp-idf` development framework. Gives raw (`unsafe`) access to drivers, Wi-Fi and more. |
| [esp-rs/esp-idf-hal]  | An implementation of the `embedded-hal` traits using the `esp-idf` framework.                                 |

[esp-rs/embedded-svc]: https://github.com/esp-rs/embedded-svc
[esp-rs/esp-idf-svc]: https://github.com/esp-rs/esp-idf-svc
[esp-rs/esp-idf-sys]: https://github.com/esp-rs/esp-idf-sys
[esp-rs/esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal

## Bare Metal (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers. It does not use `std` (the Rust standard library) but instead uses a subset, the `core` library. The [official Rust embedded book] has a [great section on this].

It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true, therefore when creating crates it's worth keeping in mind if it needs to the standard library to function.

The main focus of the `no_std` ESP support has been around the ESP32 and to a lesser extent the ESP8266. As [SVDs are released] it will be possible to create hardware abstraction layers (HAL) for newer chips.

While `esp32-hal` has great support for a lot of the onboard peripherals, Wi-Fi and Bluetooth are **not** currently supported in a `no_std` enviroment. [esp-wifi] was created to explore getting it working, but it is not yet functional.

[official rust embedded book]: https://docs.rust-embedded.org/
[great section on this]: https://docs.rust-embedded.org/book/intro/no-std.html
[svds are released]: https://github.com/espressif/svd
[esp-wifi]: https://github.com/esp-rs/esp32-wifi

### Chip Support

Chip support for `no_std` requires LLVM/Clang support just like for `std`. However, this has no dependency on `esp-idf`. In addition to compiler support, it's necessary to have peripheral access crates (PAC) and hardware abstraction layers (HAL) for your desired chip.

Refer to the table below to see if your chip is supported.

|   Chip   |   PAC   |   HAL   |
| :------: | :-----: | :-----: |
|  ESP32   |   ✅    |   ✅    |
| ESP32-C3 |   ✅    | planned |
| ESP32-S2 |   ⚠️    | planned |
| ESP32-S3 |   ⚠️    | planned |
| ESP32-H2 | planned | planned |
| ESP8266  |   ✅    |   ✅    |

**Note:** _items marked with ⚠️ have repositories, but have not yet been published to [crates.io]._

[crates.io]: https://crates.io/

### Relevant `esp-rs` Crates

| Repository           | Description                                                              |
| -------------------- | ------------------------------------------------------------------------ |
| [esp-rs/esp32-hal]   | An implementation of the `embedded-hal` traits and more for the ESP32.   |
| [esp-rs/esp32]       | A PAC crate for the ESP32.                                               |
| [esp-rs/esp32c3]     | A PAC crate for the ESP32-C3.                                            |
| [esp-rs/esp32s2]     | A PAC crate for the ESP32-S2.                                            |
| [esp-rs/esp32s3]     | A PAC crate for the ESP32-S3.                                            |
| [esp-rs/esp8266-hal] | An implementation of the `embedded-hal` traits and more for the ESP8266. |
| [esp-rs/esp8266]     | A PAC crate for the ESP8266.                                             |

[esp-rs/esp32-hal]: https://github.com/esp-rs/esp32-hal
[esp-rs/esp32]: https://github.com/esp-rs/esp32
[esp-rs/esp32c3]: https://github.com/esp-rs/esp32c3
[esp-rs/esp32s2]: https://github.com/esp-rs/esp32s2
[esp-rs/esp32s3]: https://github.com/esp-rs/esp32s3
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
[esp-rs/esp8266]: https://github.com/esp-rs/esp8266
