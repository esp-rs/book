# Required Dependencies

With an understanding of the ecosystem surrounding Rust on Espressif chips, we're able to move on to actual development. If you are not aware of the two possible development approaches or do not understand the differences between writing `std` and `no_std` applications, please first read the [Ecosystem Overview] chapter.


To build Rust applications for ESP Chips we need:
- GCC Toolchain: To serve as a C compiler.
- Rust Toolchain: Used to compile our Rust code.
  - `LLVM`: The Rust compiler uses `LLVM` as codegen backend.
- For `std` applications:
  - [ESP-IDF]: The Espressif IoT Development Framework is required in `std` project as we have seen in the [`std` overview].
  - [ldproxy] crate:  Simple tool to forward linker arguments given to [ldproxy] to the actual linker executable. The crate can be found in the [esp-rs/embuild] repository.

Let's discuss more in detail the Rust support for the architectures of the Espressif chips.

## Rust in RISC-V

The `RISC-V` architecture has support in the mainline Rust compiler so setup is relatively simple, all we must do is add the appropriate compilation target.


There are two suitable targets for this chip:

- For bare-metal (`no_std`) applications, use `riscv32imc-unknown-none-elf`
- For applications that require `std`, use `riscv32imc-esp-espidf`

The standard library target (`riscv32imc-esp-espidf`) is currently [Tier 3] and does not have prebuilt objects distributed through `rustup`.


## Rust in Xtensa
To this day, there is no `Xtensa` support in the mainline Rust compiler, for this reason, we maintain the [esp-rs/rust] fork that adds support four our `Xtensa` targets.

`Xtensa` not being supported on Rust mainline is mainly a consequence of `LLVM` not supporting `Xtensa` targets. We also maintain an LLVM fork with support for Espressif `Xtensa` targets in [espressif/llvm-project]

> #### A note in upstreaming our forks.
>
> We are trying to upstream the changes in both our `LLVM` and Rust fork.
> The first step is to upstream the `LLVM` project, this is already in progress
> and you can see the status at this [tracking issue].
> If `LLVM` changes are accepted in `LLVM` mainline, we will proceed with trying
> to upstream the Rust compiler changes.

The forked compiler can coexist with the standard Rust compiler, so it is possible to have both installed on your system. The forked compiler is invoked when using the `esp` [channel] instead of the defaults, `stable` or `nightly`.

[esp-rs/rust]: https://github.com/esp-rs/rust

In this chapter, we will cover the installation of the correct Rust compiler and toolchain.

[Ecosystem Overview]: ../overview/index.md
[ESP-IDF]: https://github.com/espressif/esp-idf
[`std` overview]: src\overview\using-the-standard-library.md
[ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-rs/embuild]: https://github.com/esp-rs/embuild
[Tier 3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3
[espressif/llvm-project]: https://github.com/espressif/llvm-project
[tracking issue]: https://github.com/espressif/llvm-project/issues/4
[channel]: https://rust-lang.github.io/rustup/concepts/channels.html
