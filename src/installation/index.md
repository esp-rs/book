# Rust on ESP targets

With an understanding of the ecosystem surrounding Rust on Espressif chips, we can move on to actual development. If you are not aware of the two possible development approaches or do not understand the differences between writing `std` and `no_std` applications, please first read the [Ecosystem Overview] chapter.

Let's take a moment to discuss the Rust support for the architectures of the Espressif chips in more detail. At this moment, Espressif SoCs are based on two different architectures: `RISC-V` and `Xtensa`. The support for those two architectures in the Rust programming language is very different.

## Rust in RISC-V targets

The `RISC-V` architecture has support in the mainline Rust compiler so setup is relatively simple, all we must do is add the appropriate compilation target.

Hence, the dependencies required to develop Rust applications in `RISC-V` targets are:

- Rust Toolchain with the proper target: Used to compile our Rust code.
- `LLVM`: Used as codegen backend by the Rust compiler.
- \[_Optional_\] `GCC` Toolchain: `GCC` linker can be used.
  - `GCC` is marked as optional as you can also use the `LLVM` linker.

Additonally, when building `std` applications we also need:

- [ESP-IDF]: Espressif IoT Development Framework.
- [`ldproxy`] crate:  Simple tool to forward linker arguments given to [`ldproxy`] to the actual linker executable. The crate can be found in the [esp-rs/embuild] repository.

## Rust in Xtensa targets

To this day, there is no `Xtensa` support in the mainline Rust compiler, for this reason, we maintain the [esp-rs/rust] fork that adds support for our `Xtensa` targets.

`Xtensa` not being supported on Rust mainline is mainly a consequence of `LLVM` not supporting `Xtensa` targets. For that reason, we also maintain an LLVM fork with support for Espressif `Xtensa` targets in [espressif/llvm-project]

Another consequence of `LLVM` not supporting our `Xtensa` targets is that we need to provide our own linker. I.e. we'll need to install [GCC toolchain] to use it as our linker.

> #### A note in upstreaming our forks
>
> We are trying to upstream the changes in both our `LLVM` and Rust forks.
> The first step is to upstream the `LLVM` project, this is already in progress
> and you can see the status at this [tracking issue].
> If our `LLVM` changes are accepted in `LLVM` mainline, we will proceed with trying
> to upstream the Rust compiler changes.

The forked compiler can coexist with the standard Rust compiler, so it is possible to have both installed on your system. The forked compiler is invoked when using the `esp` [channel] instead of the defaults, `stable` or `nightly`.

Hence, the dependencies required to develop Rust applications in `Xtensa` targets are:

- Rust Toolchain: Used to compile our Rust code.
  - _We need to use our custom fork with `Xtensa` support_.
- `LLVM`: Used as codegen backend by the Rust compiler.
  - _We need to use our custom fork with `Xtensa` support_.
- `GCC` Toolchain: `GCC` linker is used in our Rust applications as it supports all our target architectures (`Xtensa` and `RISC-V`).

Additonally, when building `std` applications we also need:

- [ESP-IDF]: Espressif IoT Development Framework.
- [`ldproxy`] crate:  Simple tool to forward linker arguments given to [`ldproxy`] to the actual linker executable. The crate can be found in the [esp-rs/embuild] repository.

[esp-rs/rust]: https://github.com/esp-rs/rust
[espressif/llvm-project]: https://github.com/espressif/llvm-project
[GCC toolchain]: https://github.com/espressif/crosstool-NG/
[tracking issue]: https://github.com/espressif/llvm-project/issues/4
[channel]: https://rust-lang.github.io/rustup/concepts/channels.html

This chapter will cover how to properly install the correct Rust compiler and toolchain for our ESP chips.

[Ecosystem Overview]: ../overview/index.md
[ESP-IDF]: https://github.com/espressif/esp-idf
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-rs/embuild]: https://github.com/esp-rs/embuild
