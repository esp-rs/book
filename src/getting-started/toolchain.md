## Rust Toolchain

### Rust Installation

Make sure you have [Rust][rust-lang-org] installed. If not, see the instructions on the [rustup][rustup.rs-website] website.

> 🚨 **Warning**: When using Unix based systems, installing Rust via a system package manager (e.g. `brew`, `apt`, `dnf`, etc.) can result in various [issues and incompatibilities][rustup-note], so it's best to use [rustup][rustup.rs-website] instead.

When using Windows, make sure you have installed one of the ABIs listed below. For more details, see the [Windows][rustup-book-windows] chapter in The rustup book.
- **MSVC**: Recommended ABI, included in the list of `rustup` default requirements. Use it for interoperability with the software produced by Visual Studio.
- **GNU**: ABI used by the GCC toolchain. Install it yourself for interoperability with the software built with the MinGW/MSYS2 toolchain.

See also [alternative installation methods][rust-alt-installation].

[rustup-note]: https://rust-lang.github.io/rustup/installation/other.html#using-a-package-manager
[rustup.rs-website]: https://rustup.rs/
[rust-alt-installation]: https://rust-lang.github.io/rustup/installation/other.html
[rustup-book-windows]: https://rust-lang.github.io/rustup/installation/windows.html
[rust-lang-org]: https://www.rust-lang.org/

### RISC-V Devices

To build Rust applications for the Espressif chips based on RISC-V architecture, do the following:

1. Install the proper toolchain with the `rust-src` [component][rustup-book-components]:
    - You can use either `stable` or [`nightly`][rustup-book-channel-nightly]:
      ```shell
      rustup toolchain install stable --component rust-src
      ```
      or
      ```shell
      rustup toolchain install nightly --component rust-src
      ```

    > ⚠️ **Note**: Other components such as `rustfmt`, `clippy` or `rust-analyzer` are not required, but they are highly recommended.



2. Install the target:
      ```shell
      rustup target add riscv32imc-unknown-none-elf # For ESP32-C2 and ESP32-C3
      rustup target add riscv32imac-unknown-none-elf # For ESP32-C6 and ESP32-H2
      ```

      Those targets are currently [Tier 2][rust-lang-book--platform-support-tier2]. Note the different flavors of `riscv32` targets in Rust covering different [RISC-V extensions][wiki-riscv-standard-extensions].

Now you should be able to build and run projects on Espressif's RISC-V chips.

[rustup-book-channel-nightly]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[rustup-book-components]: https://rust-lang.github.io/rustup/concepts/components.html
[rust-lang-book--platform-support-tier2]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[wiki-riscv-standard-extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions

### Xtensa Devices

[`espup`][espup-github] is a tool that simplifies installing and maintaining the toolchains required to develop Rust applications for the Xtensa and RISC-V architectures.

1. Install `espup`:
    ```shell
    cargo install espup --locked
    ```
   You can also directly download pre-compiled [release binaries][release-binaries] or use [`cargo-binstall`][cargo-binstall].
2. Install all necessary toolchains for all supported Espressif targets by running:
    ```shell
    espup install
    ```
   This command also installs the `stable` toolchain for RISC-V devices.

    If you want to install the toolchain only for Xtensa devices:
    ```shell
    espup install --targets esp32,esp32s2,esp32s3
    ```
    You can modify the `--targets` argument to install the toolchain for a certain device.

3. On Unix systems, set Up the Environment Variables: See the different methods in [espup Readme][source-file-espup]. Windows users don't need to do anything else.

[espup-github]: https://github.com/esp-rs/espup
[release-binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall
[source-file-espup]: https://github.com/esp-rs/espup?tab=readme-ov-file#environment-variable-setup

#### What `espup` Installs

To enable support for Espressif targets, `espup` installs:

- [Espressif Rust fork][esp-rs/rust] with support for Espressif targets
- `stable` toolchain with support for RISC-V targets
- LLVM [fork][llvm-github-fork] with support for Xtensa targets
- [GCC toolchain][gcc-toolchain-github-fork] that links the final binary

The forked compiler can coexist with the standard Rust compiler, allowing both to be installed on your system. The forked compiler is invoked when using any of the available [overriding methods][rustup-overrides].

[esp-rs/rust]: https://github.com/esp-rs/rust
[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-overrides]: https://rust-lang.github.io/rustup/overrides.html

#### Building the Rust Compiler with Xtensa Support from Source

This process is computationally expensive and can take one or more hours to complete depending on your system. It isn't recommended unless there is a major reason to go for this approach. Here is the repository to build it from source: [`esp-rs/rust` repository][esp-rs-rust].

[esp-rs-rust]: https://github.com/esp-rs/rust
