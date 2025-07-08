<p style="text-align:center;"><img src="./assets/esp-rs.svg" width="25%"></p>

# Preface

Welcome to a comprehensive guide created for embedded Rust development on Espressif pruducts. You will find everything you need to get started and build real-world applications, from setting up your development environment to writing and debugging embedded code in Rust. We will also touch on other important aspects such as our tools, give an overview of our software product architecture, and teach how you can contribute to our project.

## Who This Book Is For

This book is intended for readers with some experience in Rust and a basic understanding of embedded development and electronics. You should already be familiar with concepts such as cross-compilation, common digital interfaces (UART, SPI, I2C), memory-mapped peripherals, and interrupts. It is not a general Rust tutorial or an introduction to embedded systems — prior knowledge is expected. Check the [Additional Resources][resources] section if you are not familiar with any of these concepts. For practical examples, see the esp-hal examples see [examples].

[examples]: https://github.com/esp-rs/esp-hal/tree/main/examples
[resources]: #additional-resources

## Stability and Availability

The `esp-rs` project is under active development and may be subject to changes as it evolves. While we strive for stability, users should expect periodic modifications as we improve the API, enhance performance, and introduce new features. Modules (see the latest) that are already stabilized will not be subject to breaking changes, in accordance with semantic versioning - `SemVer`. However, `unstable` features—such as parts of the HAL and certain drivers—are actively being developed and are not covered by `SemVer` guarantees. This means that using these `unstable` components may break your project with a simple `cargo update`, much like working with Rust's `nightly` compiler. This kind of instability is common across the broader Rust embedded ecosystem, which is still rapidly evolving. Expect frequent changes and track dependencies closely.

## Additional Resources

If you're unfamiliar with certain concepts covered in this book or would like to deepen your understanding, the following resources may be helpful:

| Resource                                                 | Description                                                                           |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [The Rust Programming Language][rust-book]               | Learn Rust fundamentals before diving into embedded development.                      |
| [The Embedded Rust Book][embedded-rust-book]             | A collection of resources from Rust's Embedded Working Group.                         |
| [Embedded Rust (`no_std`) on Espressif][no_std-training] | Guide for working in `no_std` environments with Espressif SoCs.                       |
| [Awesome ESP Rust][awesome-esp-rust]                     | A list of resources for development in the Rust programming language for Espressif products |
| [Awesome Embedded Rust][awesome-embedded-rust]           | A list of resources related to embedded and low-level programming in the Rust programming language, including a selection of useful crates.                       |


[rust-book]: https://doc.rust-lang.org/book/
[embedded-rust-book]: https://docs.rust-embedded.org/book/index.html
[no_std-training]: https://esp-rs.github.io/no_std-training/
[awesome-esp-rust]: https://github.com/esp-rs/awesome-esp-rust.git
[awesome-embedded-rust]: https://github.com/rust-embedded/awesome-embedded-rust

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

We hope this book provides you with the knowledge and confidence to build robust, efficient, and safe embedded applications using Rust on Espressif products. Let's get started!

