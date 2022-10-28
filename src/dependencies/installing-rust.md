# Installing Rust

In order to develop for ESP devices using Rust you must first install the Rust compiler along with the appropriate toolchain and target(s). Depending on your device it may be one of two architectures, each requiring different setup.

If you have not yet installed Rust on your system, you can do so easily using [rustup]. For _macOS_ and _Linux_ it can be installed by runing the following command:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

For installation on Windows or alternative installation methods, please refer to the instructions on the [rustup] website.

If you are [running Windows as your host operating system, you must also install one of the available ABIs]:
- MSVC: Is the recommended ABI. When installing `rustup`, it will check if all the requirements are installed, and, if they are not, it allows the user to install them.
- GNU: No checks are done in `rustup` and expect that the user takes care of properly installing it.

[rustup]: https://rustup.rs/
[running Windows as your host operating system, you must also install one of the available ABIs]: https://rust-lang.github.io/rustup/installation/windows.html
## RISC-V
If you only want to target RISC-V chips, installation is much simpler. The bare-metal target can be installed by running:

```bash
rustup target add riscv32imc-unknown-none-elf
```
For `std` applications, the `riscv32imc-esp-espidf` target does not have prebuilt objects distributed through rustup, therefore the `-Z build-std` [unstable cargo feature] is required within your project. This [unstable cargo feature] can also be added to `.cargo/config.toml` of your project. Our [template projects] ,that we will later discuss, already take care of this.


At this point you are ready to build applications for all the Espressif chips based on RISC-V architecture.

[template projects]: /src/writing-your-application/generate-project-from-template.md
[unstable cargo feature]: https://doc.rust-lang.org/cargo/reference/unstable.html

## Xtensa

Because there is no `Xtensa` support in the mainline Rust compiler you must use the [esp-rs/rust] fork instead. There are a few options available for installing this compiler fork.

The forked compiler can coexist with the standard Rust compiler, so it is possible to have both installed on your system. The forked compiler is invoked when using the `esp` channel instead of the defaults, `stable` or `nightly`.

[esp-rs/rust]: https://github.com/esp-rs/rust

## `espup`

[esp-rs/espup] is a tool for installing and maintaining the required ecosystem to develop applications in Rust for Espressif SoC's (both `Xtensa` and `RISC-V`).

`espup` takes care of installing our forked Rust compiler, the necesary GCC toolchains for ESP chips, and many other things. For more details, [see Usage section of the `espup` Readme](https://github.com/esp-rs/espup#usage).

In order to install `espup`:
```sh
cargo install espup --git https://github.com/esp-rs/espup
```

It's also possible to directly download the pre-compiled [release binaries] or using [cargo-binstall].

[esp-rs/espup]: https://github.com/esp-rs/espup
[release binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall


Once that `espup` is installed you can simply run:
```sh
espup install
```

And it will all the necesary tools to develop Rust applications for ESP targets.

`espup` will create and export file, by default called `export-esp.sh` on Unix systems
and `export-esp.ps1` on Windows, this file contains the envionment variables that are required. Please, make sure to source in every terminal before building any application.


## Using Containers

As an alternative to installing the compiler fork to your local system directly, it's also possible to run it inside of a container.

A number of container runtimes are available, and which should be used depends on your operating system. Some of the popular options are:

- [Docker] (non-commerial use only without a license)
- [Podman]
- [Lima]

Espressif provides the [idf-rust] container image which contains several tags (generated both for `linux/arm64` and `linux/amd64`) for every Rust release:
- For `std` aplications the following naming convention is applied: `<chip>_<esp-idf-version>_<rust-toolchain-version>` . E.g., [`esp32s3_v4.4_1.64.0.0`] contains the ecosystem for developing `std` applications based on [ESP-IDF release/v4.4] for `ESP32-S3` with the `1.64.0.0` Rust toolchain.
- For `no_std` applications, the naming convention is: `<chip>_<rust-toolchain-version>`. E.g., [`esp32_1.64.0.0`] contains the ecosystem for developing `non_std` aplications for `ESP32` with the `1.64.0.0` Rust toolchain.

There is an `all` `<chip>` for both `std` and `no_std` tags which contains the enviornment required for all the ESP targets.

[docker]: https://www.docker.com/
[podman]: https://podman.io/
[lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
[`esp32s3_v4.4_1.64.0.0`]: https://hub.docker.com/layers/espressif/idf-rust/esp32s3_v4.4_1.64.0.0/images/sha256-6fa1e98d770e3edc67cbd565893aa04e5573024b1e3e373fae50907435e841e4?context=explore
[ESP-IDF release/v4.4]: https://github.com/espressif/esp-idf/tree/release/v4.4
[`esp32_1.64.0.0`]: https://hub.docker.com/layers/espressif/idf-rust/esp32_1.64.0.0/images/sha256-cc026ff9278a876f171d48978988e131940c07659485937a37cf750c44b28dfd?context=explore
