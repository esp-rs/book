# Developing on Bare Metal (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers; it does not use `std` (the Rust [`standard`][rust-lib-std] library) but instead uses a subset, the [`core`][rust-lib-core] library. [The Embedded Rust Book][embedded-rust-book] has a great [section][embedded-rust-book-no-std] on this.

It's important to note that since `no_std`uses the Rust core library, a subset of the Rust standard library,  a `no_std` crate can compile in `std` environment but the oposite is not true. Therefore, when creating crates it's worth keeping in mind if it needs the standard library to function.


[embedded-rust-book]: https://docs.rust-embedded.org/
[embedded-rust-book-no-std]: https://docs.rust-embedded.org/book/intro/no-std.html
[rust-lib-core]: https://doc.rust-lang.org/core/index.html
[rust-lib-std]: https://doc.rust-lang.org/std/index.html


## Current support

The table below covers the current support for `no_std` at this moment for different Espressif products.

> **Notes**:
>
> - ✅ - the feature is implemented or supported
> - ⏳ - the feature is under development
> - ❌ - the feature is not supported
> - [ESP8266 HAL][esp-rs/esp8266-hal] is in maintenance mode and no further development will be done for this chip

|         | [HAL][esp-rs/esp-hal] | [PAC][esp-rs/esp-pacs] | [Wi-Fi][esp-rs/esp-wifi] | [Bluetooth][esp-rs/esp-wifi] | [Heap][esp-rs/esp-alloc] | [Println][esp-rs/esp-println] | [Backtrace][esp-rs/esp-backtrace] | [Storage][esp-rs/esp-storage] |
| -------- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  ESP32   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-C2 |   ✅   |   ✅   |   ✅   |   ✅   |   ❌   |   ✅   |   ✅   |   ✅   |
| ESP32-C3 |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-C6 |   ✅   |   ✅   |   ✅   |   ⏳   |   ❌   |   ✅   |   ✅   |   ❌   |
| ESP32-S2 |   ✅   |   ✅   |   ✅   |   ❌   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-S3 |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-H2 |   ❌   |   ✅   |   ❌   |   ❌   |   ❌   |   ✅   |   ❌   |   ❌   |
| ESP8266  |   ✅   |   ✅   |   ❌   |   ❌   |   ❌   |   ✅   |   ❌   |   ❌   |

Here is a list with the relevant crates and small summary of what they do for `no_std`:

| Symbol | Repository | Description |
| ------ | ---------- | ----------- |
| **HAL** | [esp-rs/esp-hal] | Hardware abstraction layer |
| **PAC** | [esp-rs/esp-pacs] | Peripheral access crates |
| **Wi-Fi** | [esp-rs/esp-wifi] | Wi-Fi support |
| **Bluetooth** | [esp-rs/esp-wifi] | Bluetooth LE support |
| **Heap** | [esp-rs/esp-alloc] | Simple heap allocator |
| **Println** | [esp-rs/esp-println] | `print!`,  `println!` |
| **Backtrace** | [esp-rs/esp-backtrace] | Exception and panic handlers |
| **Storage**  | [esp-rs/esp-storage] | Embedded-storage traits to access unencrypted flash memory |


[esp-rs/esp-hal]: https://github.com/esp-rs/esp-hal "Hardware abstraction layer"
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
[esp-rs/esp-pacs]: https://github.com/esp-rs/esp-pacs "Peripheral access crates"
[esp-rs/esp-wifi]: https://github.com/esp-rs/esp-wifi "Wi-Fi and Bluetooth LE support"
[esp-rs/esp-alloc]: https://github.com/esp-rs/esp-alloc "Simple heap allocator"
[esp-rs/esp-println]: https://github.com/esp-rs/esp-println "print!, println!"
[esp-rs/esp-backtrace]: https://github.com/esp-rs/esp-backtrace "Exception and panic handlers"
[esp-rs/esp-storage]: https://github.com/esp-rs/esp-storage "Embedded-storage traits to access unencrypted flash memory"


