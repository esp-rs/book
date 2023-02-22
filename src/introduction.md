# Introduction

The goal of this book is to provide a comprehensive guide on using the [Rust programming language] with [Espressif] SoCs and modules.

Rust support for these devices is still a work in progress, and progress is being made rapidly. Because of this, parts of this documentation may be out of date or change dramatically between readings.

For tools and libraries relating to Rust on ESP, please see the [esp-rs organization] on GitHub.

> #### A note in SoCs support
>
> The content of the books applies to ESP32, ESP32-S, and ESP32-C series of SoCs.
> ESP8266 series is out of the scope of this book, support for Rust in ESP8266 is limited and only some parts
> of the book are applicable for this SoC.

[rust programming language]: https://www.rust-lang.org/
[Espressif]: https://espressif.com/
[esp-rs organization]: https://github.com/esp-rs/

## Status of This Book

This book is currently a work in progress. A number of sections may be missing information or be missing altogether. If there is a specific topic you would like to see documented please [open an issue].

If you feel you can contribute something to this book, we encourage you to [create a pull request]!

[open an issue]: https://github.com/esp-rs/book/issues/new
[create a pull request]: https://github.com/esp-rs/book/pulls

## Who This Book is For

This book assumes some experience with embedded development and the Rust programming language. Teaching these topics is outside the scope of this book.

If you are unfamiliar with either topic, please refer to the resources listed below to help you get started.

## Additional Resources

Some additional resources can be found below which may prove useful for those less experienced with embedded Rust.

| Resource                        | Description                                                                          |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| [The Rust Programming Language] | If you are not familiar with Rust we recommend reading this book first.              |
| [The Embedded Rust Book]        | Here you can find several other resources provided by Rust's Embedded Working Group. |
| [The Embedonomicon]             | The nitty gritty details when doing embedded programming in Rust.                    |

[the rust programming language]: https://doc.rust-lang.org/book/
[the embedded rust book]: https://docs.rust-embedded.org/book/index.html
[the embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
