# Ecosystem Overview

There are two approaches for using Rust on Espressif chips:

1. _With_ the full standard library available (`std`)
2. _Without_ the standard library available (`no_std`)

Both approaches have their advantages and disadvantages, so you should make a decision based on your project's needs. This chapter contains an overview of the two approaches followed by a brief comparison between them.

- [Using the Rust Standard Library (`std`)](./using-the-standard-library.html)
- [Bare Metal (`no_std`)](./bare-metal.html)
- [Comparing `std` and `no_std`](./comparing-std-and-no_std.html)

The [esp-rs organization] on GitHub is home to a number of repositories related to running Rust on Espressif chips. Most of the required crates have their source code hosted here.

> A note on the repository naming convention
> In the [esp-rs organization] we use the following wording:
>
> - Repositories starting with `esp-idf-` are focused on `std` approach. E.g. `esp-idf-hal`
> - Repositories starting with `esp-` are focused on `no_std` approach. E.g. `esp-hal`

[esp-rs organization]: https://github.com/esp-rs/
