<p style="text-align:center;"><img src="./assets/esp-rs.svg" width="25%"></p>

# Preface

Welcome to our guide to embedded Rust development on Espressif products. This book is designed to help you get started and become comfortable using our tools and ecosystem. Along the way, we’ll explain the reasoning behind key design choices, introduce the structure of our software stack, and walk through basic workflows using project generation and tooling. By the end, you’ll be ready to explore more advanced material through our reference documentation and external training resources.

## Who This Book Is For

This book is intended for Rust developers who are curious about embedded development, even if they don’t have prior experience with embedded systems. While some familiarity with low-level programming concepts can be helpful, we aim to introduce key ideas as they come up. If you would like to expand your baseline knowledge, consider studying the additional [Resources][resources].

## Stability and Availability

While we strive for stability, users should expect periodic modifications as we improve the API, enhance performance, and introduce new features. [Modules][modules] that are already stabilized will not be subject to breaking changes, in accordance with semantic versioning - `SemVer`. However, `unstable` features—such as parts of the `esp-hal` and certain drivers are actively being developed and are not covered by `SemVer` guarantees. This means that using these `unstable` components may break your project with a simple `cargo update`, much like working with Rust's `nightly` compiler. This kind of instability is common across the broader Rust embedded ecosystem, which is still rapidly evolving. Expect frequent changes and track dependencies closely. For all major crates, we provide migration guides between releases to help you stay up to date.


## Additional Resources

If you're unfamiliar with certain concepts covered in this book or would like to deepen your understanding, the following resources may be helpful:

| Resource                                                 | Description                                                                           |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [The Rust Programming Language][rust-book]               | Learn Rust fundamentals before diving into embedded development.                      |
| [The Embedded Rust Book][embedded-rust-book]             | A collection of resources from Rust's Embedded Working Group.                         |
| [Embedded Rust (`no_std`) on Espressif][no_std-training] | Guide for working in `no_std` environments with Espressif SoCs.                       |
| [Awesome ESP Rust][awesome-esp-rust]                     | A list of resources for development in the Rust programming language for Espressif products |
| [Awesome Embedded Rust][awesome-embedded-rust]           | A list of resources related to embedded and low-level programming in the Rust programming language, including a selection of useful crates.                       |

## Contributing to This Book

The work on this book is coordinated in [this repository][book-repository].

If you encounter difficulties following the instructions or find unclear sections, please report them in [the issue tracker][book-issues]. Contributions in the form of pull requests for typo fixes or clarity improvements are always welcome!

## Support and Community

If you need help, have questions, or would like to discuss topics related to `esp-rs`, you can reach out through the following channels:

- **Matrix**: [Join our community chat](https://matrix.to/#/#esp-rs:matrix.org).
- **GitHub Discussions**: [Engage with the community](https://github.com/esp-rs/esp-hal/discussions).
- **Email Support**: Contact the maintainers via <rust.support@espressif.com>.

We hope this book provides you with the knowledge and confidence to build robust, efficient, and safe embedded applications using Rust on Espressif products. Let's get started!

[modules]: https://docs.espressif.com/projects/rust/esp-hal/1.0.0-beta.1/esp32c6/esp_hal/index.html#modules
[resources]: #additional-resources
[rust-book]: https://doc.rust-lang.org/book/
[embedded-rust-book]: https://docs.rust-embedded.org/book/index.html
[no_std-training]: https://esp-rs.github.io/no_std-training/
[awesome-esp-rust]: https://github.com/esp-rs/awesome-esp-rust.git
[awesome-embedded-rust]: https://github.com/rust-embedded/awesome-embedded-rust
[book-repository]: https://github.com/esp-rs/book
[book-issues]: https://github.com/esp-rs/book/issues/