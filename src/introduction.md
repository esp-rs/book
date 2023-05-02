# Introduction

The goal of this book is to provide a comprehensive guide on using the [Rust Programming Language] with [Espressif] devices.

[rust programming language]: https://www.rust-lang.org/
[espressif]: https://espressif.com/

Rust support for these devices is still a work in progress, and progress is being made rapidly. Because of this, parts of this documentation may be out of date or change dramatically between readings.

For tools and libraries relating to Rust on ESP, please see the [esp-rs organization] on GitHub. This organization is managed by employees of Espressif as well as members of the community.

[esp-rs organization]: https://github.com/esp-rs/

> **A Note on Device Support**
>
> The contents of this book apply to the ESP32 series of devices only; this
> includes:
>
> - ESP32 Series
> - ESP32 C-Series
> - ESP32 S-Series
> - ESP32 H-Series
>
> The ESP8266 series is outside the scope of this book. Rust support for the
> ESP8266 series is limited and is not being officially supported by Espressif.

## Who This Book is For

This book is intended for people with some experience with Rust, and also assumes rudimentary knowledge of embedded development and electronics. For those without prior experience, we recommend first reading the [Assumptions and Prerequisites] and [Other Resources] sections to get up to speed.

[assumptions and prerequisites]: #assumptions-and-prerequisites
[other resources]: #other-resources

### Assumptions and Prerequisites

- You are comfortable using the Rust Programming Language, and have written and run applications in a desktop environment.
- You should be familiar with the idioms of the [2021 edition], as this book targets Rust 2021.
- You are comfortable developing embedded systems in another language such as C or C++, and are familiar with concepts such as:
  - Cross-compilation
  - Common digital interfaces like `UART`, `SPI`, `I²C`, etc.
  - Memory-mapped peripherals
  - Interrupts

[2021 edition]: https://doc.rust-lang.org/edition-guide/rust-2021/index.html

### Other Resources

If you are unfamiliar or less experienced with anything mentioned above, or if you would just like more information about a particular topic mentioned in this book, you may find these resources helpful.

| Resource                        | Description                                                                          |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| [The Rust Programming Language] | If you are not familiar with Rust we recommend reading this book first.              |
| [The Embedded Rust Book]        | Here you can find several other resources provided by Rust's Embedded Working Group. |
| [The Embedonomicon]             | The nitty-gritty details when doing embedded programming in Rust.                    |
| [Embedded Rust on Espressif]    | Training material created in cooperation with [Ferrous Systems].                     |

[the rust programming language]: https://doc.rust-lang.org/book/
[the embedded rust book]: https://docs.rust-embedded.org/book/index.html
[the embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
[embedded rust on espressif]: https://espressif-trainings.ferrous-systems.com/
[ferrous systems]: https://ferrous-systems.com/

### Translations

This book is currently available in English only. Once the contents of the book stabilize somewhat, we plan on translating the book into additional languages. As translations become available, this section will be updated to include them.

## How to Use This Book

This book generally assumes that you are reading it front-to-back; content covered in later chapters may not make sense without context from previous chapters.

## Contributing to This Book

The work on this book is coordinated in [this repository].

[this repository]: https://github.com/esp-rs/book

If you have trouble following the instructions in this book or find that some section of the book is not clear enough or hard to follow, then that's a bug, and it should be reported in [the issue tracker] of this book.

[the issue tracker]: https://github.com/esp-rs/book/issues/

Pull requests fixing typos and adding new content are very welcome!

## Re-using This Material

This book is distributed under the following licenses:

- The code samples and freestanding Cargo projects contained within this book are licensed under the terms of both the [MIT License] and the [Apache License v2.0].
- The written prose, pictures, and diagrams contained within this book are licensed under the terms of the Creative Commons [CC-BY-SA v4.0] license.

[mit license]: https://opensource.org/licenses/MIT
[apache license v2.0]: http://www.apache.org/licenses/LICENSE-2.0
[cc-by-sa v4.0]: https://creativecommons.org/licenses/by-sa/4.0/legalcode

TL;DR: If you want to use our text or images in your work, you need to:

- Give the appropriate credit (i.e. mention this book on your slide, and provide a link to the relevant page)
- Provide a link to the [CC-BY-SA v4.0] licence
- Indicate if you have changed the material in any way, and make any changes to our material available under the same licence

Please do let us know if you find this book useful!
