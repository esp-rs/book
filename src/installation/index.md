# Setting Up a Development Environment

With an understanding of the ecosystem surrounding Rust on Espressif chips, we can move on to actual development. If you are not aware of the two possible development approaches or do not understand the differences between writing `std` and `no_std` applications, please first read the [Ecosystem Overview] chapter.

Let's take a moment to discuss the Rust support for the different architectures of the Espressif chips and the different approaches in more detail. At this moment, Espressif SoCs are based on two different architectures: `RISC-V` and `Xtensa`. The support for those two architectures in the Rust programming language is very different.

[Ecosystem Overview]: ../overview/index.md

## Rust installation

To develop applications for ESP devices using Rust, you need to install the Rust compiler along with the appropriate toolchain and target(s). Depending on the architecture of your target(s), which can be `Xtensa` or `RISC-V`, it will require a different setup.

Make sure you have Rust installed. If not, it can easily be installed using [rustup]. For installation on Windows or [alternative installation methods], refer to the instructions on the [rustup] website.

If you are [running Windows as your host operating system, make sure you have installed one of the ABIs listed below]:
- MSVC: This is the recommended ABI and, `rustup` allows the user to install it during Rust instalaltion.
- GNU: `rustup` does not allow installting it. Useful for MinGW/MSYS2 interoperability.
For more details on Windows ABIs, see ["Windows" chapter of The rustup book].

[rustup]: https://rustup.rs/
[alternative installation methods]: https://rust-lang.github.io/rustup/installation/other.html
[running Windows as your host operating system, make sure you have installed one of the ABIs listed below]: https://rust-lang.github.io/rustup/installation/windows.html
["Windows" chapter of The rustup book]: https://rust-lang.github.io/rustup/installation/windows.html

## Rust with `std` runtime

Regardless of the target architecture (`Xtensa` or `RISC-V`), to build a project using the [`std` approach], the following tools are required:
- [`python`]: Required by ESP-IDF
- [`git`]: Required by ESP-IDF
- [`ldproxy`] crate: Simple tool to forward linker arguments given to [`ldproxy`] to the actual linker executable. To install it, use the following command:
  - `cargo install ldproxy`

The `std` runtime uses [ESP-IDF] (Espressif IoT Development Framework) as hosted environment but, users do not need to install it. It is automatically handled by [`esp-idf-sys`] (a crate that all `std` projects need to use).

[`std` approach]: ../overview/using-the-standard-library.md
[`git`]: https://git-scm.com/downloads
[`python`]: https://www.python.org/downloads/
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[ESP-IDF]: https://github.com/espressif/esp-idf

## RISC-V targets

The `RISC-V` architecture has support by the mainline Rust compiler, making the setup process relatively simple. There are two ways to proceed with the installation:
- Using the official Rust tools
- Using [`espup`, a tool that will be covered later]

If you only want to use `RISC-V` targets, you can use the official Rust tools. To build projects for `RISC-V` targets, follow these steps:
1. Install a [`nightly` toolchain] with the `rust-src` [component]:
  ```bash
  rustup toolchain install nightly --component rust-src
  ```
2. Set the target. The following two targets are recommended for most Espressif RISC-V chips:
   - For bare-metal (`no_std`) applications: `riscv32imc-unknown-none-elf`. Install it with:
       ```bash
       rustup target add riscv32imc-unknown-none-elf
       ```
   - For `std` applications: `riscv32imc-esp-espidf`. Since `riscv32imc-esp-espidf` target is currently [Tier 3], it does not have pre-built objects distributed through `rustup`, and **it does not need to be installed** as the `no_std` target.

   > #### A note in RISC-V `no_std` Rust targets.
   >
   > There are [different flavors of RISC-V 32 target in Rust] covering the different [RISC-V extensions].
3. To build `std` projects, you will also need:
- [`LLVM`] installed.
- The other requirements listed in [Rust with `std` runtime]
- To use the `-Z build-std` [unstable Cargo feature]. You can also add this [unstable Cargo feature] to `.cargo/config.toml` of your project. Our [template projects], which we will later discuss, already take care of this.

After following these steps, you will be ready to build Rust applications for all the Espressif chips based on `RISC-V` architecture.


[`espup`, a tool that will be covered later]: #espup
[`nightly` toolchain]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[component]: https://rust-lang.github.io/rustup/concepts/components.html
[template projects]: ../writing-your-own-application/generate-project-from-template.md
[unstable cargo feature]: https://doc.rust-lang.org/cargo/reference/unstable.html
[`LLVM`]: https://llvm.org/
[different flavors of RISC-V 32 target in Rust]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[RISC-V extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[Tier 3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys
[Rust with `std` runtime]: #rust-with-std-runtime

## Xtensa targets

Currently, the mainline Rust compiler does not have support for `Xtensa` targets. However, Espressif maintains the [esp-rs/rust] fork that adds support for Espressif `Xtensa` targets.

The main reason for `Xtensa` not being supported on Rust mainline is because `LLVM` does not support Xtensa targets. Therefore, Espressif also maintains a fork of `LLVM` with support for Espressif `Xtensa` targets in [espressif/llvm-project].

> #### A note in upstreaming our forks.
>
> We are making efforts to upstream the changes in Espressif `LLVM` and Rust forks.
> The first step is to upstream the `LLVM` project, this is already in progress,
> and you can see the status at this [tracking issue].
> If our `LLVM` changes are accepted in `LLVM` mainline, we will proceed with trying
> to upstream the Rust compiler changes.

Another consequence of `LLVM` not supporting Espressif `Xtensa` targets is that we need to provide our own linker. In other words, we need a [GCC toolchain] to use it as our linker.

The forked compiler can coexist with the standard Rust compiler, allowing both to be installed on your system. The forked compiler is invoked when using the `esp` [channel] instead of the defaults, `stable` or `nightly`.

To simplify the installation process, we have created a tool called `espup`.

[esp-rs/rust]: https://github.com/esp-rs/rust
[espressif/llvm-project]: https://github.com/espressif/llvm-project
[GCC toolchain]: https://github.com/espressif/crosstool-NG/
[tracking issue]: https://github.com/espressif/llvm-project/issues/4
[channel]: https://rust-lang.github.io/rustup/concepts/channels.html

### espup

[esp-rs/espup] is a `rustup`-like tool that allows you to install and update all the required components for building `std` and `no_std` Rust applications for all Espressif chips (both `Xtensa` and `RISC-V` targets).

`espup` installs the appropriate Rust compiler, either Espressif fork for `Xtensa` targets or the `nightly` toolchain with the necessary target for `RISC-V` targets, along with the required `LLVM` and `GCC` toolchains.

To install `espup`, make you have installed the [requirements], and use the following command:
```sh
cargo install espup
```
Alternatively, you can download pre-compiled [release binaries] directly or use [cargo-binstall].

After installing `espup`, simply run the following command to install all the necessary tools for developing Rust applications for all supported ESP targets:
```sh
espup install
```


`espup` generates an export file containing the required environment variables, which is saved, by default, in `$HOME/export-esp.sh` on Unix systems and `%USERPROFILE%\export-esp.ps1` on Windows.
 - On Windows, these environment variables are **automatically injected to your system during installation**.
 - On Unix systems, you need to **source the export file in every terminal before building any application**:
   ```sh
   . $HOME/export-esp.sh
   ```

For more details on `espup` commands and usage, see [Usage section] of `espup` README.

[requirements]: https://github.com/esp-rs/espup#requirements
[esp-rs/espup]: https://github.com/esp-rs/espup
[Usage section]: https://github.com/esp-rs/espup#usage
[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

### Other installation methods

In addition to using `espup` to install and update the required components for building Rust applications for Espressif chips, there are a couple of other installation methods available:

- Using [esp-rs/rust-build] installation scripts: This was the recommended way in the past, but it's now feature-frozen, and all new features will only be included in espup. For instructions on using this method, see the repository README.
- Building the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. We don't recommend this method unless there's a compelling reason to use it. For instructions, see the [Installing from Source section] of the [esp-rs/rust] repository.

[esp-rs/rust-build]: https://github.com/esp-rs/rust-build
[Installing from Source section]: https://github.com/esp-rs/rust#installing-from-source
## Using Containers

As an alternative to installing the environment directly on your local system, it's also possible to run it inside a container.

There are several container runtimes available, and the runtime you choose will depend on your operating system. Some popular options include:

- [Docker] (non-commercial use only without a license)
- [Podman]
- [Lima]

Espressif provides the [idf-rust] container image, which contains numerous tags generated both for `linux/arm64` and `linux/amd64`. For each Rust release, we generate a `<chip>_<xtensa-version>` tag containing the environment needed to develop both
`std` and `no_std` applications for the specified `<chip>`.

There is an `all` variant of `<chip>` that contains the environment required for all the ESP targets, and a `latest` variant of `<xtensa-version>` that contains the latest released Xtensa Rust toolchain.

[Docker]: https://www.docker.com/
[Podman]: https://podman.io/
[Lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
