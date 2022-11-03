# Installing Rust for Espressif SoCs

## Rust installation

In order to develop applications for ESP devices using Rust you must first install the Rust compiler along with the appropriate toolchain and target(s). Depending on your device it may be one of two architectures, each requiring a different setup.

If you have not yet installed Rust on your system, you can do so easily using [rustup]. For _macOS_ and _Linux_ it can be installed by running the following command:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

For installation on Windows or alternative installation methods, please refer to the instructions on the [rustup] website.

If you are [running Windows as your host operating system, you must also install one of the available ABIs]:
- MSVC: This is the recommended ABI. When installing `rustup`, it will check if all the requirements are installed, and, if they are not, it allows the user to install them.
- GNU: No checks are done in `rustup` and expect that the user takes care of properly installing it.

[rustup]: https://rustup.rs/
[running Windows as your host operating system, you must also install one of the available ABIs]: https://rust-lang.github.io/rustup/installation/windows.html

## ldproxy

`ldproxy` crate is required when building applications using the Rust standard library, `std`. To install:
```sh
cargo install ldproxy
```

This tool is required for `std` applications regardless of the target architecture.

[`ldproxy` crate]: https://github.com/esp-rs/embuild/tree/master/ldproxy

## RISC-V

If you only want to target `RISC-V` chips, installation is simpler. In order to build
applications for `RISC-V` targets we need to use a [Rust nightly toolchain] with the `rust-src` [component], both things can be installed with:

```bash
rustup toolchain install nightly --component rust-src
```

There are two suitable targets for this chip:

- For bare-metal (`no_std`) applications, use `riscv32imc-unknown-none-elf`
- For applications that require `std`, use `riscv32imc-esp-espidf`

The bare-metal target can be installed by running:

```bash
rustup target add riscv32imc-unknown-none-elf
```

For `std` applications, the `riscv32imc-esp-espidf` target is currently [Tier 3] and does not have prebuilt objects distributed through rustup, therefore, it does not need to be installed as `riscv32imc-unknown-none-elf`. The `-Z build-std` [unstable cargo feature] is required within your project, this [unstable cargo feature] can also be added to `.cargo/config.toml` of your project. Our [template projects], which we will later discuss, already takes care of this.

Also, when building `std` applications, make sure you have [`LLVM`] and [`ldproxy`] installed. ESP-IDF will be installed by [`esp-idf-sys`] altough you can also use an existing installation, see [ESP-IDF installation note].

At this point, you are ready to build applications for all the Espressif chips based on RISC-V architecture.

The installation of Rust for ESP `RISC-V` targets can also be handled by `espup`, a tool that will be introduced
in the [`espup` section].

[Rust nightly toolchain]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[component]: https://rust-lang.github.io/rustup/concepts/components.html
[template projects]: ../writing-your-own-application/generate-project-from-template.md
[unstable cargo feature]: https://doc.rust-lang.org/cargo/reference/unstable.html
[`LLVM`]: https://llvm.org/
[`ldproxy`]: #ldproxy
[`espup` section]: #espup
[Tier 3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3

## Xtensa

Because there is no `Xtensa` support in the mainline Rust compiler you must use the [esp-rs/rust] fork instead. There are a few options available for installing this compiler fork.

- The recommended one is using [esp-rs/espup]. See [`espup` section] for more details.
- Using [esp-rs/rust-build] installation scripts. This was the recommended way in the past, but now, the installation scripts are feature frozen and all the new features will only be included in `espup`. See the repository readme for instructions.
- Building the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system, for this reason, is not recommended unless there is a major reason to go for this approach. See instructions in the [Installing from Source section of the esp-rs/rust repository].

[esp-rs/rust]: https://github.com/esp-rs/rust
[esp-rs/rust-build]: https://github.com/esp-rs/rust-build
[Installing from Source section of the esp-rs/rust repository]: https://github.com/esp-rs/rust#installing-from-source

## espup

[esp-rs/espup] is a tool for installing and maintaining the required ecosystem to develop applications in Rust for Espressif SoC's (both `Xtensa` and `RISC-V` targets).

`espup` takes care of installing the proper Rust compiler (our fork in case of `Xtensa` targets, and the `nightly` toolchain with the necessary target for `RISC-V` targets), `LLVM` toolchain,  `GCC` toolchains, and many other things. For more details, [see Usage section of the `espup` Readme].

In order to install `espup`:
```sh
cargo install espup --git https://github.com/esp-rs/espup
```

It's also possible to directly download the pre-compiled [release binaries] or to use [cargo-binstall].

Once that `espup` is installed you can simply run:
```sh
espup install
```

And it will install all the necessary tools to develop Rust applications for all supported ESP targets.

`espup` will create and export file, by default called `export-esp.sh` on Unix systems
and `export-esp.ps1` on Windows, this file contains the required environment variables. Please, make sure to source in every terminal before building any application:

```sh
# Unix
. ./export-esp.sh
# Windows
.\export-esp.ps1
```

> #### A note in ESP-IDF installation.
> We already mentioned that [ESP-IDF is required] when building `std` applications but, so far, no
> instructions on how to install ESP-IDF have been mentioned. That is because [`esp-idf-sys`], a crate that all `std` applications
> will use, already takes care of installing the necessary ESP-IDF version. [By default, this installation will take place under an `.embuild` folder] inside
> the project directory.
>
> `espup install` has a `--espidf-version` option that allows installing the desired ESP-IDF version, see [Install subcommand instructions]. When using this argument:
> - `ldproxy` will be installed if not present already.
> - The export file will include the necessary environment variables for ESP-IDF
> - When building `std` applications that use the installed version of ESP-IDF, `esp-idf-sys` will recognize there is an activated ESP-IDF environment
> and will use it instead of downloading and installing it.
>   - This is very helpful when we want to build several projects with the same ESP-IDF version as it saves time and space.
>   - Be aware that if you source the environment with an ESP-IDF version and you try to build and `std` application that uses a different version it will fail to build.

[esp-rs/espup]: https://github.com/esp-rs/espup
[see Usage section of the `espup` Readme]: https://github.com/esp-rs/espup#usage
[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall
[ESP-IDF is required]: index.md
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys
[By default, this installation will take place under an `.embuild` folder]: https://github.com/esp-rs/esp-idf-sys#esp_idf_tools_install_dir-esp_idf_tools_install_dir
[Install subcommand instructions]: https://github.com/esp-rs/espup#install-subcommand
[ESP-IDF installation note]: #a-note-in-esp-idf-installation

## Using Containers

As an alternative to installing the compiler fork to your local system directly, it's also possible to run it inside of a container.

A number of container runtimes are available, and which should be used depends on your operating system. Some of the popular options are:

- [Docker] (non-commercial use only without a license)
- [Podman]
- [Lima]

Espressif provides the [idf-rust] container image which contains several tags (generated both for `linux/arm64` and `linux/amd64`) for every Rust release:
- For `std` applications, the following naming convention is applied: `<chip>_<esp-idf-version>_<rust-toolchain-version>` . E.g., [`esp32s3_v4.4_1.64.0.0`] contains the ecosystem for developing `std` applications based on [ESP-IDF release/v4.4] for `ESP32-S3` with the `1.64.0.0` Rust toolchain.
- For `no_std` applications, the naming convention is: `<chip>_<rust-toolchain-version>`. E.g., [`esp32_1.64.0.0`] contains the ecosystem for developing `non_std` applications for `ESP32` with the `1.64.0.0` Rust toolchain.

There is an `all` `<chip>` for both `std` and `no_std` tags that contains the environment required for all the ESP targets.

[Docker]: https://www.docker.com/
[Podman]: https://podman.io/
[Lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
[`esp32s3_v4.4_1.64.0.0`]: https://hub.docker.com/layers/espressif/idf-rust/esp32s3_v4.4_1.64.0.0/images/sha256-6fa1e98d770e3edc67cbd565893aa04e5573024b1e3e373fae50907435e841e4?context=explore
[ESP-IDF release/v4.4]: https://github.com/espressif/esp-idf/tree/release/v4.4
[`esp32_1.64.0.0`]: https://hub.docker.com/layers/espressif/idf-rust/esp32_1.64.0.0/images/sha256-cc026ff9278a876f171d48978988e131940c07659485937a37cf750c44b28dfd?context=explore
