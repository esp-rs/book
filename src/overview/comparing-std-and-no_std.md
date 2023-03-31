# Standard library (`std`) vs. Bare Metal (`no_std`)

There are several factors that must be considered when choosing between `std` and `no_std` approaches. Each approach has its own unique set of advantages and disadvantages. While we can't decide for you, this section will hopefully allow you to make an educated decision.

See also the comparison in [The Embedded Rust Book][embedded-rust-book-intro-std]

[embedded-rust-book-intro-std]: https://docs.rust-embedded.org/book/intro/no-std.html#a-no_std-rust-environment

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
