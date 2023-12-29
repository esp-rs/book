<p style="text-align:center;"><img src="./assets/esp-logo-black.svg" width="50%"></p>

# Introduction

The goal of this book is to provide a comprehensive guide on using the [Rust Programming Language][rust] with [Espressif][espressif] devices.

Rust support for these devices is still a work in progress, and progress is being made rapidly. Because of this, parts of this documentation may be out of date or change dramatically between readings.

For tools and libraries relating to Rust on ESP, please see the [esp-rs organization][esp-rs] on GitHub. This organization is managed by employees of Espressif as well as members of the community.

[rust]: https://www.rust-lang.org/
[espressif]: https://espressif.com/
[esp-rs]: https://github.com/esp-rs/

## Who This Book Is For

This book is intended for people with some experience in Rust and also assumes rudimentary knowledge of embedded development and electronics. For those without prior experience, we recommend first reading the [Assumptions and Prerequisites][prerequisites] and [Resources][resources] sections to get up to speed.

[prerequisites]: #assumptions-and-prerequisites
[resources]: #resources

### Assumptions and Prerequisites

- You are comfortable using the Rust programming language and have written and run applications in a desktop environment.
- You should be familiar with the idioms of the [2021 edition][rust-2021], as this book targets Rust 2021.
- You are comfortable developing embedded systems in another language such as C or C++, and are familiar with concepts such as:
  - Cross-compilation
  - Common digital interfaces like `UART`, `SPI`, `I2C`, etc.
  - Memory-mapped peripherals
  - Interrupts

[rust-2021]: https://doc.rust-lang.org/edition-guide/rust-2021/index.html

### Resources

If you are unfamiliar or less experienced with anything mentioned above, or if you would just like more information about a particular topic mentioned in this book. You may find these resources helpful.

| Resource                                               | Description                                                                          |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| [The Rust Programming Language][rust-book]             | If you aren't familiar with Rust we recommend reading this book first.               |
| [The Embedded Rust Book][embedded-rust-book]           | Here you can find several other resources provided by Rust's Embedded Working Group. |
| [The Embedonomicon][embedonomicon]                     | The nitty-gritty details when doing embedded programming in Rust.                    |
| [Embedded Rust (std) on Espressif][std-training]       | Getting started guide on using `std` for Espressif SoCs                              |
| [Embedded Rust (no_std) on Espressif][no_std-training] | Getting started guide on using `no_std` for Espressif SoCs                           |

[rust-book]: https://doc.rust-lang.org/book/
[embedded-rust-book]: https://docs.rust-embedded.org/book/index.html
[embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
[std-training]: https://esp-rs.github.io/std-training/
[no_std-training]: https://esp-rs.github.io/no_std-training/

## Translations

This book has been translated by generous volunteers. If you would like your translation listed here, please open a PR to add it.

- [简体中文](https://narukara.github.io/rust-on-esp-book-zh-cn/) ([repository](https://github.com/Narukara/rust-on-esp-book-zh-cn))

## How to Use This Book

This book assumes that you are reading it front-to-back. Content covered in later chapters may not make much sense without the context from previous chapters.

## Contributing to This Book

The work on this book is coordinated in [this repository][book-repository].

If you have trouble following the instructions in this book or find that some section of the book isn't clear enough, then that's a bug. Please report it in [the issue tracker][book-issues] of this book.

Pull requests fixing typos and adding new content are welcome!

[book-issues]: https://github.com/esp-rs/book/issues/
[book-repository]: https://github.com/esp-rs/book

## Re-Using This Material

This book is distributed under the following licenses:

- The code samples and freestanding Cargo projects contained within this book are licensed under the terms of both the [MIT License][mit-license] and the [Apache License v2.0][apache-license].
- The written prose, pictures, and diagrams contained within this book are licensed under the terms of the Creative Commons [CC-BY-SA v4.0][cc-license] license.

In summary, to use our text or images in your work, you need to:

- Give the appropriate credit (i.e. mention this book on your slide, and provide a link to the relevant page)
- Provide a link to the [CC-BY-SA v4.0][cc-license] license
- Indicate if you have changed the material in any way, and make any changes to our material available under the same license

Please do let us know if you find this book useful!

[mit-license]: https://opensource.org/licenses/MIT
[apache-license]: http://www.apache.org/licenses/LICENSE-2.0
[cc-license]: https://creativecommons.org/licenses/by-sa/4.0/legalcode
