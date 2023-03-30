# Ecosystem Overview

There are the following approaches to use Rust on Espressif chips:

- Using the `standard` library (`std`)
- Using the `core` library (`no_std`), a.k.a. bare metal development

Both approaches have their advantages and disadvantages, so you should make a decision based on your project's needs. This chapter contains an overview of the two approaches followed by a brief comparison between them.

- [Standard library (`std`) vs. Bare Metal (`no_std`)][rust-esp-book-std-vs-no-std]
- [Using the Standard Library (`std`)][rust-esp-book-std]
- [Developing on Bare Metal (`no_std`)][rust-esp-book-no-std]


[rust-esp-book-std-vs-no-std]: ./comparing-std-and-no_std.html
[rust-esp-book-std]: ./using-the-standard-library.html
[rust-esp-book-no-std]: ./bare-metal.html


The [esp-rs organization] on GitHub is home to a number of repositories related to running Rust on Espressif chips. Most of the required crates have their source code hosted here.

> A note on the repository naming convention
> In the [esp-rs organization] we use the folling wording:
>
> - Repositories starting with `esp-idf-` are focused on `std` apporach. E.g. `esp-idf-hal`
> - Repositories starting with `esp-` are focused on `no_std` apporach. E.g. `esp-hal`
>
> It is easy to remember as follows:
>
> - `no_std` works on top of bare metal, so `esp-` is an Espressif chip
>- `std` apart from bare metal also needs an [additional layer](https://github.com/espressif/esp-idf), which is `esp-idf-`

[esp-rs organization]: https://github.com/esp-rs/
