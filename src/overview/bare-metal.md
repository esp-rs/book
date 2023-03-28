# Developing on Bare Metal (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers; it does not use `std` (the Rust standard library) but instead uses a subset, the `core` library. [The Embedded Rust Book][embedded-rust-book] has a great [section][embedded-rust-book-no-std] on this.

It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true. Therefore, when creating crates it's worth keeping in mind if it needs the standard library to function.

[embedded-rust-book]: https://docs.rust-embedded.org/
[embedded-rust-book-no-std]: https://docs.rust-embedded.org/book/intro/no-std.html


## Relevant `esp-rs` crates

The table below covers what `no_std` features are supported by Espressif products.

> **Notes**:
>
> - Hover your cursor over the links in the table header to see feature details.
> - The icon ⏳ indicates that the feature is under development.
> - Features marked with * must be installed for `no_std` to work.

|         | [HAL][esp-rs/esp-hal]* | [PAC][esp-rs/esp-pacs]* | [Wi-Fi][esp-rs/esp-wifi] | [Bluetooth][esp-rs/esp-wifi] | [Heap][esp-rs/esp-alloc] | [Println][esp-rs/esp-println] | [Backtrace][esp-rs/esp-backtrace] | [Storage][esp-rs/esp-storage] |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  ESP32   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-C2 |   ✅   |   ✅   |   ✅   |   ✅   |   ❌   |   ✅   |   ✅   |   ✅   |
| ESP32-C3 |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-C6 |   ✅   |   ✅   |   ✅   |   ⏳   |   ❌   |   ✅   |   ✅   |   ❌   |
| ESP32-S2 |   ✅   |   ✅   |   ✅   |   ❌   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-S3 |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |   ✅   |
| ESP32-H2 |   ❌   |   ✅   |   ❌   |   ❌   |   ❌   |   ✅   |   ❌   |   ❌   |
| ESP8266  |   ✅   |   ✅   |   ❌   |   ❌   |   ❌   |   ✅   |   ❌   |   ❌   |

The `no_std` features are elaborated on below.

- **HAL** -- Hardware abstraction layer, see [esp-rs/esp-hal] and [esp-rs/esp8266-hal]
- **PAC** -- Peripheral access crates, see [esp-rs/esp-pacs]
- **Wi-Fi** -- Wi-Fi support, see [esp-rs/esp-wifi]
- **Bluetooth** -- Bluetooth LE support, see [esp-rs/esp-wifi]
- **Heap** -- Simple heap allocator, see [esp-rs/esp-alloc]
- **Println** -- `print!`, `println!`, see [esp-rs/esp-println]
- **Backtrace** -- Exception and panic handlers, see [esp-rs/esp-backtrace]
- **Storage**  -- Embedded-storage traits to access unencrypted flash memory, see [esp-rs/esp-storage]


[esp-rs/esp-hal]: https://github.com/esp-rs/esp-hal "Hardware abstraction layer"
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
[esp-rs/esp-pacs]: https://github.com/esp-rs/esp-pacs "Peripheral access crates"
[esp-rs/esp-wifi]: https://github.com/esp-rs/esp-wifi "Wi-Fi and Bluetooth LE support"
[esp-rs/esp-alloc]: https://github.com/esp-rs/esp-alloc "Simple heap allocator"
[esp-rs/esp-println]: https://github.com/esp-rs/esp-println "print!, println!"
[esp-rs/esp-backtrace]: https://github.com/esp-rs/esp-backtrace "Exception and panic handlers"
[esp-rs/esp-storage]: https://github.com/esp-rs/esp-storage "Embedded-storage traits to access unencrypted flash memory"


