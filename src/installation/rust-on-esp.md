# Rust on Espressif SoCs

Let's discuss more in detail the Rust support for the architectures of the Espressif chips. At this momment, Espressif SoCs are based on two different architectures: RISC-V and Xtensa. The support for those two architectures in the Rust programming language is very different.

## Rust in RISC-V

The `RISC-V` architecture has support in the mainline Rust compiler so setup is relatively simple, all we must do is add the appropriate compilation target.


There are two suitable targets for this chip:

- For bare-metal (`no_std`) applications, use `riscv32imc-unknown-none-elf`
- For applications that require `std`, use `riscv32imc-esp-espidf`

The standard library target (`riscv32imc-esp-espidf`) is currently [Tier 3] and does not have prebuilt objects distributed through `rustup`.


## Rust in Xtensa
To this day, there is no `Xtensa` support in the mainline Rust compiler, for this reason, we maintain the [esp-rs/rust] fork that adds support four our `Xtensa` targets.

`Xtensa` not being supported on Rust mainline is mainly a consequence of `LLVM` not supporting `Xtensa` targets. For that reason, we also maintain an LLVM fork with support for Espressif `Xtensa` targets in [espressif/llvm-project]

> #### A note in upstreaming our forks.
>
> We are trying to upstream the changes in both our `LLVM` and Rust fork.
> The first step is to upstream the `LLVM` project, this is already in progress
> and you can see the status at this [tracking issue].
> If `LLVM` changes are accepted in `LLVM` mainline, we will proceed with trying
> to upstream the Rust compiler changes.

The forked compiler can coexist with the standard Rust compiler, so it is possible to have both installed on your system. The forked compiler is invoked when using the `esp` [channel] instead of the defaults, `stable` or `nightly`.

[Tier 3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3
[esp-rs/rust]: https://github.com/esp-rs/rust
[espressif/llvm-project]: https://github.com/espressif/llvm-project
[tracking issue]: https://github.com/espressif/llvm-project/issues/4
[channel]: https://rust-lang.github.io/rustup/concepts/channels.html

