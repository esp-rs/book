# Setting Up a Development Environment

With an understanding of the ecosystem surrounding Rust on Espressif chips, we can move on to actual development. If you are not aware of the two possible development approaches or do not understand the differences between writing `std` and `no_std` applications, please first read the [Ecosystem Overview] chapter.

Let's take a moment to discuss the Rust support for the different architectures of the Espressif chips and the different approaches in more detail. At this moment, Espressif SoCs are based on two different architectures: `RISC-V` and `Xtensa`. The support for those two architectures in the Rust programming language is very different.

[Ecosystem Overview]: ../overview/index.md

## RISC-V targets

The `RISC-V` architecture has support in the mainline Rust compiler so, the setup is relatively simple. There are two ways of proceeding with the installation:
- Using the official Rust tools
- Using [`espup`, a tool that will be covered later]

If you only want to use `RISC-V` targets, you can use the official Rust tools, for this approach we need [`rustup`] installed, and a [Rust nightly toolchain] with the `rust-src` [component]. We can install a nightly toolchain with the `rust-src` component via:

```bash
rustup toolchain install nightly --component rust-src
```

These are the two recommended targets for most Espressif `RISC-V` chips:
- For bare-metal (`no_std`) applications: `riscv32imc-unknown-none-elf`
- For applications that require `std`: `riscv32imc-esp-espidf`

> #### A note in RISC-V `no_std` Rust targets.
>
> There are [different flavors of RISC-V 32 target in Rust] covering the different [RISC-V extensions].


The bare-metal targets can be installed by running:

```bash
rustup target add riscv32imc-unknown-none-elf
```

For `std` applications, the `riscv32imc-esp-espidf` target is currently [Tier 3] and does not have pre-built objects distributed through `rustup`, therefore, it does not need to be installed as the `no_std` targets. Furthermore, `std` projects, also require:
 - The `-Z build-std` [unstable cargo feature], this [unstable cargo feature] can also be added to `.cargo/config.toml` of your project. Our [template projects], which we will later discuss, already take care of this.
 - [`LLVM`] installed.
 - [`ldproxy`] installed.
 - ESP-IDF (this will be installed by automatically by [`esp-idf-sys`]).

At this point, you should be ready to build Rust applications for all the Espressif chips based on `RISC-V` architecture.

[`espup`, a tool that will be covered later]: #espup
[`rustup`]: https://rustup.rs/
[Rust nightly toolchain]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[component]: https://rust-lang.github.io/rustup/concepts/components.html
[template projects]: ../writing-your-own-application/generate-project-from-template.md
[unstable cargo feature]: https://doc.rust-lang.org/cargo/reference/unstable.html
[`LLVM`]: https://llvm.org/
[different flavors of RISC-V 32 target in Rust]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[RISC-V extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[Tier 3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys

## Xtensa targets

To this day, there is no `Xtensa` support in the mainline Rust compiler. For this reason, we maintain the [esp-rs/rust] fork that adds support for our `Xtensa` targets.

The main reason for `Xtensa` not being supported on Rust mainline is because `LLVM` does not support Xtensa targets. Therefore, we also maintain a fork of `LLVM` with support for Espressif `Xtensa` targets in [espressif/llvm-project].

> #### A note in upstreaming our forks
>
> We are trying to upstream the changes in our `LLVM` and Rust forks.
> The first step is to upstream the `LLVM` project, this is already in progress
> and you can see the status at this [tracking issue].
> If our `LLVM` changes are accepted in `LLVM` mainline, we will proceed with trying
> to upstream the Rust compiler changes.

Another consequence of `LLVM` not supporting our `Xtensa` targets is that we need to provide our own linker. In other words, we'll need to install a [GCC toolchain] to use it as our linker.

The forked compiler can coexist with the standard Rust compiler, so it is possible to have both installed on your system. The forked compiler is invoked when using the `esp` [channel] instead of the defaults, `stable` or `nightly`.

Since the installation in this scenario is slightly complex, we have created a tool called `espup` to make it easier.

[esp-rs/rust]: https://github.com/esp-rs/rust
[espressif/llvm-project]: https://github.com/espressif/llvm-project
[GCC toolchain]: https://github.com/espressif/crosstool-NG/
[tracking issue]: https://github.com/espressif/llvm-project/issues/4
[channel]: https://rust-lang.github.io/rustup/concepts/channels.html

### espup

[esp-rs/espup] is a tool for installing and maintaining the necessary ecosystem to develop applications in Rust for Espressif SoC's (both `Xtensa` and `RISC-V` targets).

`espup` takes care of installing the proper Rust compiler (our fork in case of `Xtensa` targets and the `nightly` toolchain with the necessary target for `RISC-V` targets), `LLVM` toolchain,  `GCC` toolchains, `rustup`, and many other things. For more details, [see Usage section of the `espup` Readme].

To install `espup`, use the following command:
```sh
cargo install espup
```
You can also directly download pre-compiled [release binaries] or use [cargo-binstall].

Once that `espup` is installed, you can simply run:
```sh
espup install
```

This will install all the necessary tools to develop Rust applications for all supported ESP targets.

`espup` will create an export file, by default `$HOME/export-esp.sh` on Unix systems and `%USERPROFILE%\export-esp.ps1` on Windows. This file contains some environment variables required to build projects. Make sure to source this file in every terminal before building any application:

```sh
# Unix
. $HOME/export-esp.sh
# Windows
%USERPROFILE%\export-esp.ps1
```

[esp-rs/espup]: https://github.com/esp-rs/espup
[see Usage section of the `espup` Readme]: https://github.com/esp-rs/espup#usage
[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

### Other installation methods

- Using [esp-rs/rust-build] installation scripts. This was the recommended way in the past, but now the installation scripts are feature frozen and all new features will only be included in `espup`. See the repository README for instructions.
- Building the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It is not recommended unless there is a major reason to go for this approach. See instructions in the [Installing from Source section of the esp-rs/rust repository].

[esp-rs/rust-build]: https://github.com/esp-rs/rust-build
[Installing from Source section of the esp-rs/rust repository]: https://github.com/esp-rs/rust#installing-from-source

## Rust with `std` runtime

Regardless of the target architecture, if you want to build a project using the [`std` approach], you will also need:
- [ESP-IDF]: Espressif IoT Development Framework as it's used as our hosted environment.
  - This is handled by [`esp-idf-sys`] (a crate that all `std` projects need to use) by default. See [ESP-IDF installation note] for details on how ESP-IDF can be installed.
- [`ldproxy`] crate:  Simple tool to forward linker arguments given to [`ldproxy`] to the actual linker executable. The crate can be found in the [esp-rs/embuild] repository. To install it, use the following command:
  - `cargo install ldproxy`


[ESP-IDF]: https://github.com/espressif/esp-idf
[`std` approach]: ../overview/using-the-standard-library.md
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-rs/embuild]: https://github.com/esp-rs/embuild
[ESP-IDF installation note]: https://github.com/esp-rs/espup#esp-idf-instalation

## Using Containers

As an alternative to installing the environment directly on your local system, it's also possible to run it inside a container.

A number of container runtimes are available, and which one you should use depends on your operating system. Some popular options are:

- [Docker] (non-commercial use only without a license)
- [Podman]
- [Lima]

Espressif provides the [idf-rust] container image which contains several tags (generated both for `linux/arm64` and `linux/amd64`) for every Rust release:
- For `std` applications, the following naming convention is applied: `<chip>_<esp-idf-version>_<rust-toolchain-version>`. For example, [`esp32s3_v4.4_1.64.0.0`] contains the ecosystem for developing `std` applications based on [ESP-IDF release/v4.4] for `ESP32-S3` with the `1.64.0.0` Rust toolchain.
- For `no_std` applications, the naming convention is: `<chip>_<rust-toolchain-version>`. For example, [`esp32_1.64.0.0`] contains the ecosystem for developing `non_std` applications for `ESP32` with the `1.64.0.0` Rust toolchain.

There is an `all` variant of `<chip>` that contains the environment required for all the ESP targets, and a `latest` variant of `<rust-toolchain-version>` that contains the latest Xtensa Rust toolchain.

[Docker]: https://www.docker.com/
[Podman]: https://podman.io/
[Lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
[`esp32s3_v4.4_1.64.0.0`]: https://hub.docker.com/layers/espressif/idf-rust/esp32s3_v4.4_1.64.0.0/images/sha256-6fa1e98d770e3edc67cbd565893aa04e5573024b1e3e373fae50907435e841e4?context=explore
[ESP-IDF release/v4.4]: https://github.com/espressif/esp-idf/tree/release/v4.4
[`esp32_1.64.0.0`]: https://hub.docker.com/layers/espressif/idf-rust/esp32_1.64.0.0/images/sha256-cc026ff9278a876f171d48978988e131940c07659485937a37cf750c44b28dfd?context=explore
