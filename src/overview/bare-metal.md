# Bare Metal (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers; it does not use `std` (the Rust standard library) but instead uses a subset, the `core` library. The [official Rust embedded book] has a [great section on this].

It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true. Therefore, when creating crates it's worth keeping in mind if it needs to the standard library to function.

[official rust embedded book]: https://docs.rust-embedded.org/
[great section on this]: https://docs.rust-embedded.org/book/intro/no-std.html
[svds are released]: https://github.com/espressif/svd
[esp-wifi]: https://github.com/esp-rs/esp32-wifi

## Chip Support

The main focus of the `no_std` ESP support has been around the ESP32 and to a lesser extent the ESP8266. As [SVDs are released] it will be possible to create hardware abstraction layers (HAL) for newer chips.

While `esp32-hal` and `esp8266-hal` have great support for a lot of the onboard peripherals, Wi-Fi and Bluetooth are **not** currently supported in a `no_std` enviroment. [esp-wifi] was created to explore getting it working, but it is not yet functional.

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

## Relevant `esp-rs` Crates

| Repository           | Description                                                                                      |
| -------------------- | ------------------------------------------------------------------------------------------------ |
| [esp-rs/esp32-hal]   | An implementation of the `embedded-hal` traits and more for the ESP32 using bare metal (no_std). |
| [esp-rs/esp32]       | A PAC crate for the ESP32.                                                                       |
| [esp-rs/esp32c3]     | A PAC crate for the ESP32-C3.                                                                    |
| [esp-rs/esp32s2]     | A PAC crate for the ESP32-S2.                                                                    |
| [esp-rs/esp32s3]     | A PAC crate for the ESP32-S3.                                                                    |
| [esp-rs/esp8266-hal] | An implementation of the `embedded-hal` traits and more for the ESP8266.                         |
| [esp-rs/esp8266]     | A PAC crate for the ESP8266.                                                                     |

[esp-rs/esp32-hal]: https://github.com/esp-rs/esp32-hal
[esp-rs/esp32]: https://github.com/esp-rs/esp32
[esp-rs/esp32c3]: https://github.com/esp-rs/esp32c3
[esp-rs/esp32s2]: https://github.com/esp-rs/esp32s2
[esp-rs/esp32s3]: https://github.com/esp-rs/esp32s3
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
[esp-rs/esp8266]: https://github.com/esp-rs/esp8266
