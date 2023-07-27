# Writing no_std applications
If you want to learn how to develop `no_std` application, see the following training materials:
- The book [Embedded Rust (no_std) on Espressif][no-std-book]
- The repository [esp-rs/no_std-training][no-std-repository]

The training is based on [ESP32-C3-DevKit-RUST-1][esp-rust-board]. You can use any other Espressif development board but code changes and configuration changes might be needed.

The training contains:
* Introductory level examples:
   * A basic hello-world ([Source][hello-world])
   * A panic example ([Source][panic])
   * A blinky example ([Source][blinky])
   * A button example ([Source][button])
   * A button with interrupt example ([Source][button-interrupt])

> Note that there are several examples covering the use of specific peripherals under the examples' folder of every SoC [`esp-hal`][esp-hal]. E.g. [`esp32c3-hal/examples`][esp32c3-hal-examples]

[no-std-book]: https://esp-rs.github.io/no_std-training/
[no-std-repository]: https://github.com/esp-rs/no_std-training
[esp-rust-board]: https://github.com/esp-rs/esp-rust-board
[hello-world]: https://github.com/esp-rs/no_std-training/tree/main/intro/hello-world
[panic]: https://github.com/esp-rs/no_std-training/tree/main/intro/panic
[blinky]: https://github.com/esp-rs/no_std-training/tree/main/intro/blinky
[button]: https://github.com/esp-rs/no_std-training/tree/main/intro/button
[button-interrupt]: https://github.com/esp-rs/no_std-training/tree/main/intro/button-interrupt
[esp-hal]: https://github.com/esp-rs/esp-hal
[esp32c3-hal-examples]: https://github.com/esp-rs/esp-hal/tree/main/esp32c3-hal/examples
