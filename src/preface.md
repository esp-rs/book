<p style="text-align:center;"><img src="./assets/esp-logo-black.svg" width="50%"></p>

# Preface

Welcome to a comprehensive guide to using the `esp-rs` driver, created for embedded Rust development on Espressif products. This book serves as a practical resource for developers wishing to utilise the capabilities of Rust in `no_std` environments when working with Espressif hardware platforms.

## Who This Book Is For

This book is intended for people with some experience in Rust and assumes a basic knowledge of embedded development and electronics. For those without prior experience, we recommend first reading the [Scope of This Book][prerequisites] and [Resources][resources] sections to get up to speed.

[prerequisites]: #scope-of-this-book
[resources]: #additional-resources

## Scope of This Book

In this book, we focus specifically on the `esp-rs` driver and its ecosystem. It does **not** serve as:

- A general Rust programming tutorial: if you are new to Rust, we recommend starting with [The Rust Programming Language][rust-book].
- An embedded development guide: prior familiarity with embedded systems concepts such as cross-compilation, common digital interfaces (UART, SPI, I2C), memory-mapped peripherals and interrupts is assumed.

## Stability and Availability

The `esp-rs` driver is under active development and may be subject to changes as it evolves. While we strive for stability, users should expect periodic modifications as we improve the API, improve performance, and introduce new features. Those [modules] that are already stabilised will not be subject to frequent dramatic changes, while the `unstable` parts of the driver are being actively worked on and will likely undergo changes in future versions of the driver. 

Additionally, the ecosystem of Rust itself in a broader meaning is still in its infancy. Some dependencies, tools, or language features may change over time. We encourage developers to stay up to date with the latest releases and participate in discussions to help shape the future of Rust on Espressif platforms.

[modules]: https://docs.espressif.com/projects/rust/esp-hal/1.0.0-beta.0/esp32c6/esp_hal/index.html#modules

## Additional Resources

If you're unfamiliar with certain concepts covered in this book or would like to deepen your understanding, the following resources may be helpful:

| Resource                                               | Description                                                                          |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| [The Rust Programming Language][rust-book]             | Learn Rust fundamentals before diving into embedded development.                     |
| [The Embedded Rust Book][embedded-rust-book]           | A collection of resources from Rust's Embedded Working Group.                        |
| [The Embedonomicon][embedonomicon]                     | Detailed insights into low-level embedded Rust programming.                          |
| [Embedded Rust (std) on Espressif][std-training]       | Getting started guide for using Rust's `std` environment on Espressif SoCs.         |
| [Embedded Rust (no_std) on Espressif][no_std-training] | Guide for working in `no_std` environments with Espressif SoCs.                     |

[rust-book]: https://doc.rust-lang.org/book/
[embedded-rust-book]: https://docs.rust-embedded.org/book/index.html
[embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
[std-training]: https://esp-rs.github.io/std-training/
[no_std-training]: https://esp-rs.github.io/no_std-training/

## Contributing to This Book

The work on this book is coordinated in [this repository][book-repository].

If you encounter difficulties following the instructions or find unclear sections, please report them in [the issue tracker][book-issues]. Contributions in the form of pull requests for typo fixes or clarity improvements are always welcome!

[book-repository]: https://github.com/esp-rs/book
[book-issues]: https://github.com/esp-rs/book/issues/

## Support and Community

If you need help, have questions, or would like to discuss topics related to `esp-rs`, you can reach out through the following channels:

- **Matrix**: [Join our community chat](https://matrix.to/#/#esp-rs:matrix.org)
- **GitHub Discussions**: [Engage with the community](https://github.com/esp-rs/esp-hal/discussions)
- **Email Support**: Contact the maintainers via <rust.support@espressif.com>

We hope this book provides you with the knowledge and confidence to build robust, efficient, and safe embedded applications using Rust on Espressif devices. Let's get started!

