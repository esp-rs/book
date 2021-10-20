# Ecosystem overview

As it stands there are two approaches for using Rust on Espressif chips, `std` and `no_std`. Both approaches have thier pros and cons, you should choose the approach based on your project's needs.

## Using the Rust standard library

Espressif provides a C based development framework called [`esp-idf`] which has support for Espressif chips from the ESP32 and later. `esp-idf` provides a [newlib] environment with enough functionality to build the Rust standard library (`std`) on top of it, hence this approach. The supported `std` features are as follows.

- threads
- mutexes and other synchronization primitives
- collections
- random number generation
- sockets

On top of `std` features, there is a [`embedded-svc`] implementation for `esp-idf` which adds extra support for services/modules not available in the standard library, including:

- WiFi management
- NVS (non-volatile storage)
- Networking services like `httpd` and `ping`

[`esp-idf`]: https://github.com/espressif/esp-idf
[newlib]: https://sourceware.org/newlib/
[`embedded-svc`]: https://github.com/esp-rs/embedded-svc

### Chip Support

Chip support for `std` requires two things, LLVM/Clang support and support in `esp-idf`.

| chip    | supported |
| ------- | --------- |
| esp32   | yes       |
| esp32c3 | yes       |
| esp32s2 | yes       |
| esp32s3 | planned   |
| esp32h2 | planned   |
| esp8266 | **no**    |

### Relevant `esp-rs` crates

| repo                  | description                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------------------------- |
| [esp-rs/embedded-svc] | Abstraction traits for embedded services.                                                                  |
| [esp-rs/esp-idf-svc]  | An implementation of [embedded-svc] using `esp-idf` drivers.                                               |
| [esp-rs/esp-idf-sys]  | Rust bindings to the `esp-idf` development framework. Gives raw (unsafe) access to drivers, WiFi and more. |
| [esp-rs/esp-idf-hal]  | An implementation of the `embedded-hal` traits using the `esp-idf` framework.                              |

[esp-rs/embedded-svc]: https://github.com/esp-rs/embedded-svc
[esp-rs/esp-idf-svc]: https://github.com/esp-rs/esp-idf-svc
[esp-rs/esp-idf-sys]: https://github.com/esp-rs/esp-idf-sys
[esp-rs/esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal

## `no_std`

Using `no_std` may be more familiar to embedded Rust developers. It does not use `std` (the Rust standard library) but instead uses a subset, the `core` library. The [official rust embedded book] has a [great section on this].

It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true, therefore when creating crates it's worth keeping in mind if it needs to the standard library to function.

The main focus of the `no_std` ESP support has been around the ESP32 and to a lesser extent the ESP8266. As [SVDs are released] it will be possible to create hardware abstraction layers (HAL) for newer chips. While `esp32-hal` has great support for a lot of the onboard peripherals, WiFi and bluetooth are **not** currently supported in a `no_std` enviroment. [esp-wifi] was created to explore getting it working, but it is not yet functional.

[official rust embedded book]: https://docs.rust-embedded.org/
[great section on this]: https://docs.rust-embedded.org/book/intro/no-std.html
[svds are released]: https://github.com/espressif/svd
[esp-wifi]: https://github.com/esp-rs/esp32-wifi

### Relevant `esp-rs` crates

| repo                 | description                                                              |
| -------------------- | ------------------------------------------------------------------------ |
| [esp-rs/esp32-hal]   | An implementation of the `embedded-hal` traits and more for the ESP32.   |
| [esp-rs/esp32]       | A PAC crate for the ESP32.                                               |
| [esp-rs/esp32c3]     | A PAC crate for the ESP32C3.                                             |
| [esp-rs/esp8266-hal] | An implementation of the `embedded-hal` traits and more for the ESP8266. |
| [esp-rs/esp8266]     | A PAC crate for the ESP8266.                                             |

[esp-rs/esp32-hal]: https://github.com/esp-rs/esp32-hal
[esp-rs/esp32]: https://github.com/esp-rs/esp32
[esp-rs/esp32c3]: https://github.com/esp-rs/esp32c3
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
[esp-rs/esp8266]: https://github.com/esp-rs/esp8266
