# Wokwi
[Wokwi] is an online simulator that supports simulating Rust projects (both `std` and `no_std`) in ESP Chips,
see [wokwi.com/rust] for a list of examples and a way to start new projects.

Wokwi offers WiFi simulation, Virtual Logic Analyzer, and GDB debugging among many other features, see
[Wokwi documentation] for more details. For ESP chips, there is a [table of simulation features that are currently supported].

[Wokwi]: https://wokwi.com/
[wokwi.com/rust]: https://wokwi.com/rust
[Wokwi documentation]: https://docs.wokwi.com/
[table of simulation features that are currently supported]: https://docs.wokwi.com/guides/esp32#simulation-features

## Using Wokwi Server
[Wokwi server] is a CLI tool for launching a Wokwi simulation of your project. I.e., it allows you
to build a project on your machine, or in a container, and simulate the resulting binary.

[Wokwi server]: https://github.com/MabezDev/wokwi-server
