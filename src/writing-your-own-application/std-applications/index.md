# Writing std applications

If you want to learn how to develop `std` application, there is a training developed
alongside [Ferrous Systems]:

- [Book of training]
- [Repository of the training]

The training is based on [ESP32-C3-DevKit-RUST-1]. You can use any other ESP32, ESP32-C3, ESP32-S2, or ESP32-S3 development board but code changes and configuration changes might be needed.

The training is split into two parts:

* [Introductory level examples]:
   * A basic hardware-check ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/hardware-check))
   * An HTTP Client ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/http-client))
   * An HTTP Server ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/http-server))
   * An MQTT Client ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/intro/mqtt))
* [Advanced level examples]:
   * Low-level GPIO
   * Interrupts in General
   * I2C Driver ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced/i2c-driver))
   * I2C Sensor Reading ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced/i2c-sensor-reading))
   * GPIO/Button Interrupts ([Source](https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced/button-interrupt))
   * Driving an RGB LED


> Note that there are several examples covering the use of specific peripherals under the examples folder of  `esp-idf-hal`. I.e. [`esp32-idf-hal/examples`].

[Ferrous Systems]: https://ferrous-systems.com/
[Book of training]: https://espressif-trainings.ferrous-systems.com/
[Repository of the training]: https://github.com/ferrous-systems/espressif-trainings
[ESP32-C3-DevKit-RUST-1]: https://github.com/esp-rs/esp-rust-board
[Introductory level examples]: https://github.com/ferrous-systems/espressif-trainings/tree/main/intro
[Advanced level examples]: https://github.com/ferrous-systems/espressif-trainings/tree/main/advanced
[`esp32-idf-hal/examples`]: https://github.com/esp-rs/esp-idf-hal/tree/master/examples
