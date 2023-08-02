# The Rust on ESP Book

This book is a work in progress.

## Quickstart

This book is generated using [`mdbook`] and additionally uses the [`mdbook-mermaid`] preprocessor to add support for diagrams. To install these tools:

```shell
cargo install mdbook mdbook-mermaid
```

With `mdbook` and `mdbook-mermaid` installed, you can clone the repository and start a development server by running:

```shell
git clone https://github.com/esp-rs/book
cd book/
mdbook serve
```

[`mdbook`]: https://github.com/rust-lang/mdBook
[`mdbook-mermaid`]: https://github.com/badboy/mdbook-mermaid

## License

The Rust on ESP Book is distributed under the following licenses:

- The code samples contained within this book are licensed under the terms of
  both the [MIT License] and the [Apache License v2.0].
- The written prose contained within this book is licensed under the terms of
  the Creative Commons [CC-BY-SA v4.0] license.

[mit license]: ./LICENSE-MIT
[apache license v2.0]: ./LICENSE-APACHE
[cc-by-sa v4.0]: ./LICENSE-CC-BY-SA

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the
work by you, as defined in the Apache-2.0 license, shall be licensed as above, without any
additional terms or conditions.

While contributing, please follow the [Rust Documentation Style Guide](rust-doc-style-guide.md).
