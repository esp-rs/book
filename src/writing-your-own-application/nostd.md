# Writing no_std applications
If you want to learn how to develop `no_std` application, see the following training materials:
- The book [Embedded Rust (no_std) on Espressif]
- The repository [esp-rs/no_std-training]

The training is based on [ESP32-C3-DevKit-RUST-1]. You can use any other Espressif development board but code changes and configuration changes might be needed.

The training contains:
* Introductory level examples:
   * A basic hello-world ([Source](https://github.com/esp-rs/no_std-training/tree/main/intro/hello-world))
   * A panic example ([Source](https://github.com/esp-rs/no_std-training/tree/main/intro/panic))
   * A blinky example ([Source](https://github.com/esp-rs/no_std-training/tree/main/intro/blinky))
   * A button example ([Source](https://github.com/esp-rs/no_std-training/tree/main/intro/button))
   * A button with interrupt example ([Source](https://github.com/esp-rs/no_std-training/tree/main/intro/button-interrupt))

> Note that there are several examples covering the use of specific peripherals under the examples' folder of every SoC [`esp-hal`]. E.g. [`esp32c3-hal/examples`]

[Embedded Rust (no_std) on Espressif]: https://esp-rs.github.io/no_std-training/
[esp-rs/no_std-training]: https://github.com/esp-rs/no_std-training
[ESP32-C3-DevKit-RUST-1]: https://github.com/esp-rs/esp-rust-board
[`esp-hal`]: https://github.com/esp-rs/esp-hal
[`esp32c3-hal/examples`]: https://github.com/esp-rs/esp-hal/tree/main/esp32c3-hal/examples
