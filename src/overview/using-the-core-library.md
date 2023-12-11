# Using the Core Library (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers. It doesn't use `std` (the Rust [`standard`][rust-lib-std] library) but instead uses a subset, the [`core`][rust-lib-core] library. [The Embedded Rust Book][embedded-rust-book] has a great [section][embedded-rust-book-no-std] on this.

It is important to note that `no_std` uses the Rust `core` library. As this library is part of the Rust `standard` library, a `no_std` crate can compile in `std` environment. However, the opposite isn't true: an `std` crate can't compile in `no_std` environment. This information is worth remembering when deciding which library to choose.

[embedded-rust-book]: https://docs.rust-embedded.org/
[embedded-rust-book-no-std]: https://docs.rust-embedded.org/book/intro/no-std.html
[rust-lib-core]: https://doc.rust-lang.org/core/index.html
[rust-lib-std]: https://doc.rust-lang.org/std/index.html

## Current Support

The table below covers the current support for `no_std` at this moment for different Espressif products.

|          | [HAL][esp-hal] | [Wi-Fi/BLE/ESP-NOW][esp-wifi] | [Backtrace][esp-backtrace] | [Storage][esp-storage] |
| -------- | :------------: | :---------------------------: | :------------------------: | :--------------------: |
| ESP32    |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-C2 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-C3 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-C6 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-H2 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-S2 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-S3 |       ✅        |               ✅               |             ✅              |           ✅            |

> ⚠️ **Note**:
>
> - ✅ in Wi-Fi/BLE/ESP-NOW means that the target supports, at least, one of the listed technologies. For details, see [Current support][esp-wifi-current-support] table of the esp-wifi repository.
> - [ESP8266 HAL][esp8266-hal] is in maintenance mode and no further development will be done for this chip.

[esp-hal]: https://github.com/esp-rs/esp-hal "Hardware abstraction layer"
[esp-wifi]: https://github.com/esp-rs/esp-wifi "Wi-Fi, BLE and ESP-NOW support"
[esp-backtrace]: https://github.com/esp-rs/esp-backtrace "Exception and panic handlers"
[esp-storage]: https://github.com/esp-rs/esp-storage "Embedded-storage traits to access unencrypted flash memory"
[esp-wifi-current-support]: https://github.com/esp-rs/esp-wifi#current-support
[esp8266-hal]: https://github.com/esp-rs/esp8266-hal "ESP8266 Hardware abstraction layer"

### Relevant `esp-rs` Crates

| Repository                       | Description                                                |
| -------------------------------- | ---------------------------------------------------------- |
| [`esp-hal`][esp-hal]             | Hardware abstraction layer                                 |
| [`esp-pacs`][esp-pacs]           | Peripheral access crates                                   |
| [`esp-wifi`][esp-wifi]           | Wi-Fi, BLE and ESP-NOW support                             |
| [`esp-alloc`][esp-alloc]         | Simple heap allocator                                      |
| [`esp-println`][esp-println]     | `print!`,  `println!`                                      |
| [`esp-backtrace`][esp-backtrace] | Exception and panic handlers                               |
| [`esp-storage`][esp-storage]     | Embedded-storage traits to access unencrypted flash memory |

### When You Might Want to Use the Core Library (`no_std`)

- Small memory footprint: If your embedded system has limited resources and needs to have a small memory footprint, you will likely want to use bare-metal because `std` features add a significant amount of final binary size and compilation time.
- Direct hardware control: If your embedded system requires more direct control over the hardware, such as low-level device drivers or access to specialized hardware features you will likely want to use bare-metal because `std` adds abstractions that can make it harder to interact directly with the hardware.
- Real-time constraints or time-critical applications: If your embedded system requires real-time performance or low-latency response times because `std` can introduce unpredictable delays and overhead that can affect real-time performance.
- Custom requirements: bare-metal allows more customization and fine-grained control over the behavior of an application, which can be useful in specialized or non-standard environments.

[esp-pacs]: https://github.com/esp-rs/esp-pacs "Peripheral access crates"
[esp-alloc]: https://github.com/esp-rs/esp-alloc "Simple heap allocator"
[esp-println]: https://github.com/esp-rs/esp-println "print!, println!"
