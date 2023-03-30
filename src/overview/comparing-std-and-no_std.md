# Standard library (`std`) vs. Bare Metal (`no_std`)

## Support for Espressif Products

|   Chip   | `std` | `no_std` |
| -------- | :--------: | :---: |
|  ESP32   |     ✅      |     ✅      |
| ESP32-C2 |     ⏳      |     ✅      |
| ESP32-C3 |     ✅      |     ✅      |
| ESP32-C6 |      ?      |     ✅      |
| ESP32-S2 |     ✅      |     ✅      |
| ESP32-S3 |     ✅      |     ✅      |
| ESP32-H2 |     ⏳      |     ⏳      |
| ESP8266  |     ❌      |     ✅      |

The products supported in certain circumstances will be called _supported Espressif products_ throughout the book.

As of now, the Espressif products supported by the esp-idf framework are the ones supported for Rust `std` development. For details on different versions of esp-idf and support of Espressif chips, see [this table][esp-idf-release-compatibility].

[esp-idf-release-compatibility]: https://github.com/espressif/esp-idf#esp-idf-release-and-soc-compatibility/

## Software Stacks

`std`

- Rust `std` library
- [newlib][newlib-env]
- esp-idf

`no_std`

- Rust `core` library
- peripheral access crates (PAC)
- hardware abstraction layers (HAL)

In both cases, LLVM/Clang is required for compilation.

[newlib-env]: https://sourceware.org/newlib/
