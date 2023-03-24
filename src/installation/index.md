# Setting Up a Development Environment

At the moment, Espressif SoCs are based on two different architectures -- `RISC-V` and `Xtensa`. Each architecture supports the `std` and `no_std` application development.

To set up the development environment, do the following:

1. [Install Rust][install-rust]
2. Install requirements for the targets
    - [RISC-V targets][risc-v-targets]
    - [Xtensa targets][xtensa-targets]

As mentioned in the installation procedures below, for `std` development also don't forget to install [`std` Development Requirements][rust-esp-book-std-requirements].

Please note that you can host the development environment with support for Xtensa in a [container][use-containers].


[install-rust]: #install-rust
[risc-v-targets]: #risc-v-targets
[xtensa-targets]: #xtensa-targets
[use-containers]: #using-containers


## Install Rust

Make sure you have Rust installed. If not, see the instructions on the [rustup][rustup.rs-website] website.

See also [alternative installation methods][rust-alt-installation].

> **Note**: If you run Windows on your host machine, make sure you have installed one of the ABIs listed below. For more details, see the rustup book > [Chapter _Windows_][rustup-book-windows]
>
> - **MSVC**: Recommended ABI, included in the list of `rustup` default requirements. Use it for interoperability with the software produced by Visual Studio.
> - **GNU**: ABI used by the GCC toolchain. Install it yourself for interoperability with the software built with the MinGW/MSYS2 toolchain.


[rustup.rs-website]: https://rustup.rs/
[rust-alt-installation]: https://rust-lang.github.io/rustup/installation/other.html
[rustup-book-windows]: https://rust-lang.github.io/rustup/installation/windows.html


## RISC-V targets

To build Rust applications for the Espressif chips based on `RISC-V` architecture, do the following:

1. Install the [`nightly`][rustup-book-channel-nightly] toolchain with the `rust-src` [component][rustup-book-components]:

    ```bash
    rustup toolchain install nightly --component rust-src
    ```

[rustup-book-channel-nightly]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[rustup-book-components]: https://rust-lang.github.io/rustup/concepts/components.html


2. Set the target:
    - For `no_std` (bare-metal) applications, run:

      ```bash
      rustup target add riscv32imc-unknown-none-elf
      ```

      This target is currently [Tier 2][rust-lang-book--platform-support-tier2]; note the different flavors of `riscv32` target in Rust covering different [`RISC-V` extensions][wiki-riscv-standard-extensions].

    - For `std` applications, use the target `riscv32imc-esp-espidf`.

      Since this target is currently [Tier 3][rust-lang-book--platform-support-tier3], it does not have pre-built objects distributed through `rustup`, and **it does not need to be installed** as the `no_std` target.


[rust-lang-book--platform-support-tier2]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[wiki-riscv-standard-extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[rust-lang-book--platform-support-tier3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3


3. To build `std` projects, you also need to install:
    - [`LLVM`][llvm-website] compiler infrastructure
    - Other [`std` developemnt requirements][rust-esp-book-std-requirements]
    - In your project's file `.cargo/config.toml`, add the unstable Cargo [feature][cargo-book-unstable-features] `-Z build-std`. Our [template projects][rust-esp-book-write-app-generate-project] that are discussed later in this book already have it added.


[llvm-website]: https://llvm.org/
[rust-esp-book-std-requirements]: #std-development-requirements
[cargo-book-unstable-features]: https://doc.rust-lang.org/cargo/reference/unstable.html
[rust-esp-book-write-app-generate-project]: ../writing-your-own-application/generate-project-from-template.md


Now you should be able to build and run projects on the Espressif's `RISC-V` chips.


## Xtensa targets

[espup][espup-github] is a tool that simplifies installing and maintaining the components required to develop Rust applications for the `Xtensa` architecture.

[espup-github]: https://github.com/esp-rs/espup

To install `espup`, run:
```sh
cargo install espup
```

You can also directly download pre-compiled [release binaries] or use [cargo-binstall].

[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

Once `espup` is installed, do the following:

1. Install all the necessary tools to develop Rust applications for all supported Espressif targets by running:
    ```sh
    espup install
    ```

    `espup` will create an export file that contains some environment variables required to build projects:

    - Unix systems - `$HOME/export-esp.sh`
    - Windows - `%USERPROFILE%\export-esp.ps1`

2. Make sure to source this file in every terminal before building any application:

    - Unix systems, run `. $HOME/export-esp.sh`
    - Windows does not require sourcing any file


After installing `espup`

- `no_std` (bare-metal) applications should work out of the box
- `std` applications require additional software covered in [`std` Development Requirements][rust-esp-book-std-requirements]

### Other installation methods

- Using [esp-rs/rust-build] installation scripts. This was the recommended way in the past, but now the installation scripts are feature frozen and all new features will only be included in `espup`. See the repository README for instructions.
- Building the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It is not recommended unless there is a major reason to go for this approach. See instructions in the [Installing from Source section of the esp-rs/rust repository].

[esp-rs/rust-build]: https://github.com/esp-rs/rust-build
[Installing from Source section of the esp-rs/rust repository]: https://github.com/esp-rs/rust#installing-from-source

### What espup Installs

To enable support for Xtensa targets, `espup` installs the following tools:

- Espressif Rust fork for `Xtensa` targets
- `nightly` toolchain with the necessary `RISC-V` targets
- `LLVM` [fork][llvm-github-fork] that supports `Xtensa` targets
- [GCC toolchain][gcc-toolchain-github-fork] as it is used as linker

The forked compiler can coexist with the standard Rust compiler, allowing both to be installed on your system. The forked compiler is invoked when using the `esp` [channel][rustup-github-concepts-channel] instead of the defaults, `stable` or `nightly`.

> **Note**: We are making efforts to upstream our forks

> 1. Changes in `LLVM` fork. Already in progress, see the status in this [tracking issue][llvm-github-fork-upstream issue].
> 2. Rust compiler forks. If `LLVM` changes are accepted, we will proceed with the Rust compiler changes.


[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-github-concepts-channel]: https://rust-lang.github.io/rustup/concepts/channels.html
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4


## `std` Development Requirements

Regardless of the target architecture make sure you have the following required tools installed to build [`std`][rust-esp-book-overview-std] applications

- [`python`][python-website-download]: Required by ESP-IDF
- [`git`][git-website-download]: Required by ESP-IDF
- [`ldproxy`][embuild-github-ldproxy] crate: Simple tool to pass the `ldproxy` linker arguments to the actual linker executable. Install it  by running
  - `cargo install ldproxy`

Required by all `std` projects and included in the project's file `.cargo/config.toml`, the crate [`esp-idf-sys`] also automatically downloads and installs ESP-IDF (Espressif IoT Development Framework).


[rust-esp-book-overview-std]: ../overview/using-the-standard-library.md
[python-website-download]: https://www.python.org/downloads/
[git-website-download]: https://git-scm.com/downloads
[embuild-github-ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys-github]: https://github.com/esp-rs/esp-idf-sys
[esp-idf-github]: https://github.com/espressif/esp-idf


## Using Containers

Instead of installing directly on your local system, you can host the development environment inside a container. Espressif provides the [idf-rust] image that supports both `RISC-V` and `Xtensa` target architectures and enables both `std` and `no_std` development.

You can find numerous tags for `linux/arm64` and `linux/amd64`.

For each Rust release, we generate the tag with the following naming conventions

- `std` applications
  - `<chip>_<esp-idf-version>_<rust-toolchain-version>`
  - For example, `esp32s3_v4.4_1.64.0.0` contains the ecosystem for developing `std` applications based on [ESP-IDF release/v4.4] for `ESP32-S3` with the `1.64.0.0` Rust toolchain.
- `no_std` applications
  - `<chip>_<rust-toolchain-version>`
  - For example, `esp32_1.64.0.0` contains the ecosystem for developing `no_std` applications for `ESP32` with the `1.64.0.0` Rust toolchain

There are special cases

- `<chip>` can be `all` which indicates compatibility with all ESP targets
- `<rust-toolchain-version>` can be `latest` which indicates the latest release of the `Xtensa` Rust toolchain

Depending on your operating system, you can choose any container runtime, such as [Docker], [Podman], or [Lima].


[Docker]: https://www.docker.com/
[Podman]: https://podman.io/
[Lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
