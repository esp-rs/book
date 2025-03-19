<p style="text-align:center;"><img src="./assets/esp-rs.svg" width="25%"></p>

# Preface

Welcome to a comprehensive guide to using the `esp-rs` ecosystem, created for embedded Rust development on Espressif products. You will find everything you need to get started and build real-world applications, from setting up your development environment to writing and debugging embedded code in Rust. We will also touch on other important aspects such as our tools, give an overview of our software product architecture, and teach how you can contribute to our project. 

## Who This Book Is For

This book is intended for people with some experience in Rust and assumes basic knowledge of embedded development and electronics. Readers of this book are expected to have a basic understanding of concepts such as cross-compilation, common digital interfaces (UART, SPI, I2C), memory-mapped peripherals and interrupts. For those without prior experience, we recommend first reading the [Scope of This Book][prerequisites] and [Resources][resources] sections to get up to speed.

[prerequisites]: #scope-of-this-book

## Scope of This Book

In this book, we focus specifically on the `esp-rs` ecosystem. It does **not** serve as:

- A general Rust programming language tutorial: Familiarity with the Rust language is assumed to get you started. 
- An embedded development guide: Prior familiarity with basic embedded development is also assumed.
- A collection of examples: This book explains the overall process of working with the `esp-rs` ecosystem. More examples can be found [here][examples]. 

Check the [Additional Resources][resources] section if you are not familiar with any of these concepts.

[examples]: https://github.com/esp-rs/esp-hal/tree/main/examples
[resources]: #additional-resources

## Stability and Availability

The `esp-rs` project is under active development and may be subject to changes as it evolves. While we strive for stability, users should expect periodic modifications as we improve the API, improve performance, and introduce new features. [Modules] that are already stabilized will not be subject to breaking changes, while the `unstable` parts of the drivers are being actively worked on and will likely undergo changes in future releases. 

Additionally, the embedded Rust ecosystem is still in its infancy; some dependencies, tools, or language features may change over time. We encourage developers to stay up to date with the latest releases and participate in discussions to help shape the future of Rust on Espressif platforms.

[modules]: https://docs.espressif.com/projects/rust/esp-hal/1.0.0-beta.0/esp32c6/esp_hal/index.html#modules

## Additional Resources

If you're unfamiliar with certain concepts covered in this book or would like to deepen your understanding, the following resources may be helpful:

| Resource                                               | Description                                                                          |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| [The Rust Programming Language][rust-book]             | Learn Rust fundamentals before diving into embedded development.                     |
| [The Embedded Rust Book][embedded-rust-book]           | A collection of resources from Rust's Embedded Working Group.                        |
| [The Embedonomicon][embedonomicon]                     | Detailed insights into low-level embedded Rust programming.                          |
| [Embedded Rust (no_std) on Espressif][no_std-training] | Guide for working in `no_std` environments with Espressif SoCs.                     |
| [Awesome ESP Rust][awesome-esp-rust]                   | A list of resouces for development in the Rust programming language for ESP products | 

[rust-book]: https://doc.rust-lang.org/book/
[embedded-rust-book]: https://docs.rust-embedded.org/book/index.html
[embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
[no_std-training]: https://esp-rs.github.io/no_std-training/
[awesome-esp-rust]: https://github.com/esp-rs/awesome-esp-rust.git

## Contributing to This Book

The work on this book is coordinated in [this repository][book-repository].

If you encounter difficulties following the instructions or find unclear sections, please report them in [the issue tracker][book-issues]. Contributions in the form of pull requests for typo fixes or clarity improvements are always welcome!

[book-repository]: https://github.com/esp-rs/book
[book-issues]: https://github.com/esp-rs/book/issues/

## Support and Community

If you need help, have questions, or would like to discuss topics related to `esp-rs`, you can reach out through the following channels:

- **Matrix**: [Join our community chat](https://matrix.to/#/#esp-rs:matrix.org).
- **GitHub Discussions**: [Engage with the community](https://github.com/esp-rs/esp-hal/discussions).
- **Email Support**: Contact the maintainers via <rust.support@espressif.com>.

We hope this book provides you with the knowledge and confidence to build robust, efficient, and safe embedded applications using Rust on Espressif devices. Let's get started!

