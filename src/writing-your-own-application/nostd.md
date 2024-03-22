# Writing `no_std` Applications
If you want to learn how to develop `no_std` application, see the following training materials:
- The book [Embedded Rust (`no_std`) on Espressif][no-std-book]
- The repository [`no_std-training`][no-std-repository]

The training is based on [ESP32-C3-DevKit-RUST-1][esp-rust-board]. You can use any other Espressif development board but code changes and configuration changes might be needed.

The training contains:
* Introductory level examples:
   * [A basic hello-world][hello-world]
   * [A panic example][panic]
   * [A blinky example][blinky]
   * [A button example][button]
   * [A button with an interrupt example][button-interrupt]

> ⚠️ **Note**: There are several examples covering the use of specific peripherals under the [`examples`][esp-hal-examples] folder [`esp-hal`][esp-hal]. For running instructions and device compatibility for a given example, refer to the [`examples` README][examples-readme].



[no-std-book]: https://esp-rs.github.io/no_std-training/
[no-std-repository]: https://github.com/esp-rs/no_std-training
[esp-rust-board]: https://github.com/esp-rs/esp-rust-board
[hello-world]: https://github.com/esp-rs/no_std-training/tree/main/intro/hello-world
[panic]: https://github.com/esp-rs/no_std-training/tree/main/intro/panic
[blinky]: https://github.com/esp-rs/no_std-training/tree/main/intro/blinky
[button]: https://github.com/esp-rs/no_std-training/tree/main/intro/button
[button-interrupt]: https://github.com/esp-rs/no_std-training/tree/main/intro/button-interrupt
[esp-hal]: https://github.com/esp-rs/esp-hal
[esp-hal-examples]: https://github.com/esp-rs/esp-hal/tree/main/examples
