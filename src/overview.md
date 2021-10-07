# Ecosystem overview

As it stands there are two approaches for using Rust on Espressif chips, `std` and `no_std`. Both approaches have thier pro's and con's, you should choose the approach based on your project needs.

## Using the Rust standard library

Espressif provides a C based development framework called [`esp-idf`](https://github.com/espressif/esp-idf) which has support for Espressif chips from the esp32 and later. `esp-idf` provides a newlib environment with enough functionality to build the Rust standard library (`std`) on top of it, hence this approach. The supported `std` features are as follows.

* threads
* mutexes and other synchronization primitives
* collections
* rng
* sockets

On top of `std` features, there is a `embedded-svc` implementation for `esp-idf` which adds extra support for services/modules not available in the standard library, including:

* WiFi management
* NVS storage
* Networking services like `httpd` & `ping`

### Chip support 

Chip support for `std` requires two things, LLVM/Clang support & support in `esp-idf`.

|chip | supported |
|-|-|
| esp32 | yes |
| esp32c3 | yes |
| esp32s2 | yes |
| esp32s3 | planned |
| esp32h2 | planned |
| esp8266 | no |

### Relevant `esp-rs` crates

|repo|description|
|-|-|
| [esp-rs/embedded-svc](https://github.com/esp-rs/embedded-svc) | Abstraction traits for embedded services. |
| [esp-rs/esp-idf-svc](https://github.com/esp-rs/esp-idf-svc)   | An implementation of [embedded-svc](https://github.com/esp-rs/embedded-svc) using `esp-idf` drivers. |
| [esp-rs/esp-idf-sys](https://github.com/esp-rs/esp-idf-sys)   | Rust bindings to the `esp-idf` development framework. Gives raw (unsafe) access to drivers, WiFi and more. |
| [esp-rs/esp-idf-hal](https://github.com/esp-rs/esp-idf-hal)   | An implementation of the `embedded-hal` traits using the `esp-idf` framework. |

## `no_std`

Using `no_std` may be more familiar to embedded Rust developers. It does not use `std` instead it uses a subset, the `core` library. The [official rust embedded book](https://docs.rust-embedded.org/) has a [great section on this](https://docs.rust-embedded.org/book/intro/no-std.html). It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true, therefore when creating crates it's worth keeping in mind if it needs to the standard library to function.

The main focus of the `no_std` esp support has been around the `esp32` and to a lesser extent the `esp8266`. As [SVD's are released](https://github.com/espressif/svd) it will be possible to create hardware abstraction layers (HAL) for newer chips. Whilst the `esp32-hal` has great support for a lot of the onboard peripherals, WiFi and bluetooth are **not** currently supported in a `no_std` enviroment. [esp-wifi](https://github.com/esp-rs/esp32-wifi) was created to explore getting it working, but it is not yet functional.

### Relevant `esp-rs` crates

|repo|description|
|-|-|
| [esp-rs/esp32-hal](https://github.com/esp-rs/esp32-hal) | An implementation of the `embedded-hal` traits and more for the esp32. |
| [esp-rs/esp32](https://github.com/esp-rs/esp32) | A PAC crate for the esp32. |
| [esp-rs/esp32c3](https://github.com/esp-rs/esp32c3) | A PAC crate for the esp32c3. |
| [esp-rs/esp8266-hal](https://github.com/esp-rs/esp8266-hal) | An implementation of the `embedded-hal` traits and more for the esp8266. |
| [esp-rs/esp8266](https://github.com/esp-rs/esp8266) | A PAC crate for the esp8266. |
