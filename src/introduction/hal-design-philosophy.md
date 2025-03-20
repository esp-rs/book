# HAL Design Philosophy

The HAL was designed based on some recommended patterns and rules that are described in several documents:
- [`esp-hal` developers' guidelines][guidelines]
- [The Embedded Rust Book - HAL Design Patterns][embedded-rust-patterns]

[guidelines]: https://github.com/esp-rs/esp-hal/blob/main/documentation/DEVELOPER-GUIDELINES.md
[embedded-rust-patterns]: https://docs.rust-embedded.org/book/design-patterns/hal/index.html

The HAL implements blocking APIs for all peripherals, and asynchronous APIs for the peripherals which support asynchronous operation. When applicable, drivers implement the [`embedded-hal`][embedded-hal] and [`embedded-hal-async`][embedded-hal-async] traits.

## `Peripheral` Pattern

`esp-hal` drivers take pins and peripherals as `peripheral::Peripheral` in most circumstances. This means you can pass the pin/peripheral or a mutable reference to the pin/peripheral.

[embedded-hal]: https://docs.rs/embedded-hal/latest/embedded_hal/
[embedded-hal-async]: https://docs.rs/embedded-hal-async/latest/embedded_hal_async/
