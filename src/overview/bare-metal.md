# Bare Metal (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers; it does not use `std` (the Rust standard library) but instead uses a subset, the `core` library. The [official Rust embedded book] has a [great section on this].

It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true. Therefore, when creating crates it's worth keeping in mind if it needs the standard library to function.

[great section on this]: https://docs.rust-embedded.org/book/intro/no-std.html
[official rust embedded book]: https://docs.rust-embedded.org/

## Hardware Abstraction Layers

Previously, the primary focus of `no_std` development was the ESP32 and (to a lesser extent) the ESP8266.

Now there is a renewed effort to implement `no_std` support for the entire lineup of Espressif devices from the ESP32 and newer. These new HALs can be found in the [esp-rs/esp-hal] repository.

There is also some level of support for Wi-Fi and Bluetooth via [esp-rs/esp-wifi] for ESP32, ESP32-C3, ESP32-S2, and ESP32-S3.

[esp-rs/esp-hal]: https://github.com/esp-rs/esp-hal
[esp-rs/esp-wifi]: https://github.com/esp-rs/esp-wifi

## Chip Support

Chip support for `no_std` requires LLVM/Clang support just like for `std`. However, this has no dependency on `esp-idf`. In addition to compiler support, it's necessary to have peripheral access crates (PAC) and hardware abstraction layers (HAL) for your desired chip.

Refer to the table below to see if your chip is supported. Please note that the `no_std` HALs are still in the early phases of development, so not all peripherals have had drivers implemented.

|   Chip   |    PAC    |    HAL    |
| :------: | :-------: | :-------: |
|  ESP32   |    ✅     |    ✅     |
| ESP32-C2 |    ✅     |    ✅     |
| ESP32-C3 |    ✅     |    ✅     |
| ESP32-C6 | _planned_ | _planned_ |
| ESP32-S2 |    ✅     |    ✅     |
| ESP32-S3 |    ✅     |    ✅     |
| ESP32-H2 | _planned_ | _planned_ |
| ESP8266  |    ✅     |    ✅     |

## Relevant `esp-rs` Crates

| Repository           | Description                                                                                                        |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [esp-rs/esp-pacs]    | A monorepo containing PACs for each supported device.                                                              |
| [esp-rs/esp-hal]     | An implementation of the `embedded-hal` traits and more for the ESP32, ESP32-C2, ESP32-C3, ESP32-S2, and ESP32-S3. |
| [esp-rs/esp8266-hal] | An implementation of the `embedded-hal` traits and more for the ESP8266.                                           |

[esp-rs/esp-pacs]: https://github.com/esp-rs/esp-pacs
[esp-rs/esp-hal]: https://github.com/esp-rs/esp-hal
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
