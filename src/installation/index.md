# Setting Up a Development Environment

At the moment, Espressif SoCs are based on two different architectures: `RISC-V` and `Xtensa`. Both architectures support `std` and `no_std` approaches.

To set up the development environment, do the following:

1. [Install Rust][install-rust]
2. Install requirements based on your target(s)
    - [`RISC-V` targets only][risc-v-targets]
    - [`RISC-V` and `Xtensa` targets][rics-v-xtensa-targets]

As mentioned in the installation procedures below, for `std` development also don't forget to install [`std` Development Requirements][rust-esp-book-std-requirements].

Please note that you can host the development environment in a [container][use-containers].


[install-rust]: #install-rust
[risc-v-targets]: #risc-v-targets-only
[rics-v-xtensa-targets]: #risc-v-and-xtensa-targets
[use-containers]: #using-containers


## Install Rust

Make sure you have [Rust][rust-lang-org] installed. If not, see the instructions on the [rustup][rustup.rs-website] website.

When using Windows, make sure you have installed one of the ABIs listed below. For more details, see the [Windows][rustup-book-windows] chapter in The rustup book.
- **MSVC**: Recommended ABI, included in the list of `rustup` default requirements. Use it for interoperability with the software produced by Visual Studio.
- **GNU**: ABI used by the GCC toolchain. Install it yourself for interoperability with the software built with the MinGW/MSYS2 toolchain.

> **Warning**: When using Unix based systems, installing Rust via a system package manager (e.g. `brew`, `apt`, `dnf`, etc.) can result in various issues and incompatibilities, so it's best to use [rustup][rustup.rs-website] instead.

See also [alternative installation methods][rust-alt-installation].

[rustup.rs-website]: https://rustup.rs/
[rust-alt-installation]: https://rust-lang.github.io/rustup/installation/other.html
[rustup-book-windows]: https://rust-lang.github.io/rustup/installation/windows.html
[rust-lang-org]: https://www.rust-lang.org/


## RISC-V targets only

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
      rustup target add riscv32imc-unknown-none-elf # For ESP32-C2 and ESP32-C3
      rustup target add riscv32imac-unknown-none-elf # For ESP32-C6 and ESP32-H2
      ```

      This target is currently [Tier 2][rust-lang-book--platform-support-tier2]; note the different flavors of `riscv32` target in Rust covering different [`RISC-V` extensions][wiki-riscv-standard-extensions].

    - For `std` applications, use the target `riscv32imc-esp-espidf`.

      Since this target is currently [Tier 3][rust-lang-book--platform-support-tier3], it does not have pre-built objects distributed through `rustup`, and **it does not need to be installed** as the `no_std` target.


[rust-lang-book--platform-support-tier2]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[wiki-riscv-standard-extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[rust-lang-book--platform-support-tier3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3


3. To build `std` projects, you also need to install:
    - [`LLVM`][llvm-website] compiler infrastructure
    - Other [`std` development requirements][rust-esp-book-std-requirements]
    - In your project's file `.cargo/config.toml`, add the unstable Cargo [feature][cargo-book-unstable-features] `-Z build-std`. Our [template projects][rust-esp-book-write-app-generate-project] that are discussed later in this book already include this.


[llvm-website]: https://llvm.org/
[rust-esp-book-std-requirements]: #std-development-requirements
[cargo-book-unstable-features]: https://doc.rust-lang.org/cargo/reference/unstable.html
[rust-esp-book-write-app-generate-project]: ../writing-your-own-application/generate-project-from-template.md


Now you should be able to build and run projects on the Espressif's `RISC-V` chips.


## RISC-V and Xtensa targets

[`espup`][espup-github] is a tool that simplifies installing and maintaining the components required to develop Rust applications for the `Xtensa` and `RISC-V` architectures.

[espup-github]: https://github.com/esp-rs/espup

### 1. Install `espup`

To install `espup`, run:
```sh
cargo install espup
```

You can also directly download pre-compiled [release binaries] or use [cargo-binstall].

[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

### 2. Install neccesary toolchains

Install all the necessary tools to develop Rust applications for all supported Espressif targets by running:
```sh
espup install
```

> **Note**: `std` applications require installing additional software covered in [`std` Development Requirements][rust-esp-book-std-requirements]

### 3. Set up the environment variables
`espup` will create an export file that contains some environment variables required to build projects.

On Windows (`%USERPROFILE%\export-esp.ps1`)
  - There is **no need** to execute the file for Windows users. It is only created to show the modified environment variables.

On Unix based systems (`$HOME/export-esp.sh`). There are different ways of sourcing the file:
- Source this file in every terminal:
   1. Source the export file: `. $HOME/export-esp.sh`

   This approach requires running the command in every new shell.
- Create an alias for executing the `export-esp.sh`:
   1. Copy and paste the following command to your shell’s profile (`.profile`, `.bashrc`, `.zprofile`, etc.): `alias get_esprs='. $HOME/export-esp.sh'`
   2. Refresh the configuration by restarting the terminal session or by running `source [path to profile]`, for example, `source ~/.bashrc`.

   This approach requires running the alias in every new shell.
- Add the environment variables to your shell's profile directly:
   1. Add the content of `$HOME/export-esp.sh` to your shell ’s profile: `cat $HOME/export-esp.sh >> [path to profile]`, for example, `cat $HOME/export-esp.sh >> ~/.bashrc`.
   2. Refresh the configuration by restarting the terminal session or by running `source [path to profile]`, for example, `source ~/.bashrc`.

   This approach **does not** require any sourcing. The `export-esp.sh` script will be sourced automatically in every shell.


### What espup Installs

To enable support for Espressif targets, `espup` installs the following tools:

- Espressif Rust fork with support for Espressif targets
- `nightly` toolchain with support for `RISC-V` targets
- `LLVM` [fork][llvm-github-fork] with support for `Xtensa` targets
- [GCC toolchain][gcc-toolchain-github-fork] that links the final binary

The forked compiler can coexist with the standard Rust compiler, allowing both to be installed on your system. The forked compiler is invoked when using any of the available [overriding methods][rustup-overrides].

> **Note**: We are making efforts to upstream our forks
> 1. Changes in `LLVM` fork. Already in progress, see the status in this [tracking issue][llvm-github-fork-upstream issue].
> 2. Rust compiler forks. If `LLVM` changes are accepted, we will proceed with the Rust compiler changes.


[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-overrides]: https://rust-lang.github.io/rustup/overrides.html
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4


### Other installation methods for Xtensa targets

- Using [esp-rs/rust-build] installation scripts. This was the recommended way in the past, but now the installation scripts are feature frozen, and all new features will only be included in `espup`. See the repository README for instructions.
- Building the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It is not recommended unless there is a major reason to go for this approach. Here is the repository to build it from source: [esp-rs/rust repository].

[esp-rs/rust-build]: https://github.com/esp-rs/rust-build#download-installer-in-bash
[esp-rs/rust repository]: https://github.com/esp-rs/rust


## `std` Development Requirements

Regardless of the target architecture, make sure you have the following required tools installed to build [`std`][rust-esp-book-overview-std] applications:

- [`python`][python-website-download]: Required by ESP-IDF
- [`git`][git-website-download]: Required by ESP-IDF
- [`ldproxy`][embuild-github-ldproxy] binary crate: A tool that forwards linker arguments to the actual linker that is also given as an argument to `ldproxy`. Install it by running:
    ```sh
    cargo install ldproxy
    ```

The std runtime uses [ESP-IDF][esp-idf-github] (Espressif IoT Development Framework) as hosted environment but, users do not need to install it. ESP-IDF is automatically downloaded and installed by [esp-idf-sys][esp-idf-sys-github], a crate that all std projects need to use, when building a std application.


[rust-esp-book-overview-std]: ../overview/using-the-standard-library.md
[python-website-download]: https://www.python.org/downloads/
[git-website-download]: https://git-scm.com/downloads
[embuild-github-ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys-github]: https://github.com/esp-rs/esp-idf-sys
[esp-idf-github]: https://github.com/espressif/esp-idf


## Using Containers

Instead of installing directly on your local system, you can host the development environment inside a container. Espressif provides the [idf-rust] image that supports both `RISC-V` and `Xtensa` target architectures and enables both `std` and `no_std` development.

You can find numerous tags for `linux/arm64`, and `linux/amd64` platforms.

For each Rust release, we generate the tag with the following naming convention:

- `<chip>_<rust-toolchain-version>`
  - For example, `esp32_1.64.0.0` contains the ecosystem for developing `std`, and `no_std` applications for `ESP32` with the `1.64.0.0` Xtensa Rust toolchain.

There are special cases

- `<chip>` can be `all` which indicates compatibility with all Espressif targets
- `<rust-toolchain-version>` can be `latest` which indicates the latest release of the `Xtensa` Rust toolchain

Depending on your operating system, you can choose any container runtime, such as [Docker], [Podman], or [Lima].


[Docker]: https://www.docker.com/
[Podman]: https://podman.io/
[Lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
