# Overview of Development Approaches

There are the following approaches to using Rust on Espressif chips:

- Using the `std` library, a.k.a. Standard library.
- Using the `core` library (`no_std`), a.k.a. bare metal development.

Both approaches have their advantages and disadvantages, so you should make a decision based on your project's needs. This chapter contains an overview of the two approaches:

- [Using the Standard Library (`std`)][rust-esp-book-std]
- [Using the Core Library (`no_std`)][rust-esp-book-no-std]

See also the comparison of the different runtimes in [The Embedded Rust Book][embedded-rust-book-intro-std].

The [esp-rs organization] on GitHub is home to several repositories related to running Rust on Espressif chips. Most of the required crates have their source code hosted here.

[rust-esp-book-std]: ./using-the-standard-library.md
[rust-esp-book-no-std]: ./using-the-core-library.md
[embedded-rust-book-intro-std]: https://docs.rust-embedded.org/book/intro/no-std.html#a-no_std-rust-environment
[esp-rs organization]: https://github.com/esp-rs/

## Repository Naming Convention

In the [esp-rs organization], we use the following wording:
- Repositories starting with `esp-` are focused on `no_std` approach. For example, `esp-hal`
  - `no_std` works on top of bare metal, so `esp-` is an Espressif chip
- Repositories starting with `esp-idf-` are focused on `std` approach. For example, `esp-idf-hal`
  - `std`, apart from bare metal, also needs an [additional layer], which is `esp-idf-`

[additional layer]: https://github.com/espressif/esp-idf

## Support for Espressif Products

> ⚠️ **Notes**:
>
> - ✅ - The feature is implemented or supported
> - ⏳ - The feature is under development
> - ❌ - The feature isn't supported

| Chip     | `std` | `no_std` |
| -------- | :---: | :------: |
| ESP32    |   ✅   |    ✅     |
| ESP32-C2 |   ✅   |    ✅     |
| ESP32-C3 |   ✅   |    ✅     |
| ESP32-C6 |   ✅   |    ✅     |
| ESP32-S2 |   ✅   |    ✅     |
| ESP32-S3 |   ✅   |    ✅     |
| ESP32-H2 |   ✅   |    ✅     |
| ESP8266  |   ❌   |    ✅     |

> ⚠️ **Note**: The ESP8266 series is outside the scope of this book. Rust support for the
> ESP8266 series is limited and isn't being officially supported by Espressif.

The products supported in certain circumstances will be called _supported Espressif products_ throughout the book.
