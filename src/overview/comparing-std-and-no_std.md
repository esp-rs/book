Standard library (`std`) vs. Bare Metal (`no_std`)

## Support for Espressif Products

|   Chip   | `std` | `no_std` |
| -------- | :--------: | :---: |
|  ESP32   |     ✅      |     ✅      |
| ESP32-C2 | _planned_   |     ✅      |
| ESP32-C3 |     ✅      |     ✅      |
| ESP32-C6 |      ?      | _planned_  |
| ESP32-S2 |     ✅      |     ✅      |
| ESP32-S3 |     ✅      |     ✅      |
| ESP32-H2 | _planned_   | _planned_  |
| ESP8266  |     ❌      |     ✅      |

The products supported in certain circumstances will be called _supported Espressif products_ throughout the book.

As of now, the Espressif products supported by the esp-idf framework are the ones supported for Rust `std` development.


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
