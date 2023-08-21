# Writing `std` Applications

If you want to learn how to develop `std` application, see the following training materials developed alongside [Ferrous Systems][ferrous-systems]:
- The book [Embedded Rust on Espressif][std-book]
- The repository [`std-training`][std-repository]

The training is based on [ESP32-C3-DevKit-RUST-1][esp-rust-board]. You can use any other Espressif development board, but code changes and configuration changes might be needed.

The training is split into two parts:

* [Introductory level examples][intro]:
   * [A basic hardware-check][hardware-check]
   * [An HTTP Client][http-client]
   * [An HTTP Server][http-server]
   * [An MQTT Client][mqtt]
* [Advanced level examples][advanced]:
   * Low-level GPIO
   * Interrupts in General
   * [I2C Driver][i2c-driver]
   * [I2C Sensor Reading][i2c-sensor-reading]
   * [GPIO/Button Interrupts][button-interrupt]
   * Driving an RGB LED

> ⚠️ **Note**: There are several examples covering the use of specific peripherals under the examples' folder of  [`esp-idf-hal`][esp-idf-hal]. I.e. [`esp-idf-hal/examples`][esp-idf-hal-examples].

[ferrous-systems]: https://ferrous-systems.com/
[std-book]: https://esp-rs.github.io/std-training/
[std-repository]: https://github.com/esp-rs/std-training
[esp-rust-board]: https://github.com/esp-rs/esp-rust-board
[intro]: https://github.com/esp-rs/std-training/tree/main/intro
[hardware-check]: https://github.com/esp-rs/std-training/tree/main/intro/hardware-check
[http-client]: https://github.com/esp-rs/std-training/tree/main/intro/http-client
[http-server]: https://github.com/esp-rs/std-training/tree/main/intro/http-server
[mqtt]: https://github.com/esp-rs/std-training/tree/main/intro/mqtt
[advanced]: https://github.com/esp-rs/std-training/tree/main/advanced
[i2c-driver]: https://github.com/esp-rs/std-training/tree/main/advanced/i2c-driver
[i2c-sensor-reading]: https://github.com/esp-rs/std-training/tree/main/advanced/i2c-sensor-reading
[button-interrupt]: https://github.com/esp-rs/std-training/tree/main/advanced/button-interrupt
[esp-idf-hal-examples]: https://github.com/esp-rs/esp-idf-hal/tree/master/examples
[esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal
