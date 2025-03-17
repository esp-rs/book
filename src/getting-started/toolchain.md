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
[embedonomicon-creating-a-custom-target]: https://docs.rust-embedded.org/embedonomicon/custom-target.html
[embedonomicon-official-book]: https://docs.rust-embedded.org/embedonomicon/

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
    This also install the `stable` toolchain for RISC-V devices, if you want to install the toolchain only for Xtensa devices:
    ```shell
    espup install --targets esp32,esp32s2,esp32s3
    ```
    You can modify the `--targets` argument to install the toolchain for a certain device.

3. Set Up the Environment Variables: `espup` will create an export file that contains some environment variables required to build projects.
   - On Windows (`%USERPROFILE%\export-esp.ps1`)
        - There is **no need** to execute the file for Windows users. It is only created to show the modified environment variables.
   - On Unix-based systems (`$HOME/export-esp.sh`). There are different ways of sourcing the file:
     - Source this file in every terminal:
        1. Source the export file: `. $HOME/export-esp.sh`

        This approach **requires running the command in every new shell**.
     - Create an alias for executing the `export-esp.sh`:
        1. Copy and paste the following command to your shell’s profile (`.profile`, `.bashrc`, `.zprofile`, etc.): `alias get_esprs='. $HOME/export-esp.sh'`
        2. Refresh the configuration by restarting the terminal session or by running `source [path to profile]`, for example, `source ~/.bashrc`.

        This approach **requires running the alias in every new shell**.
     - Add the environment variables to your shell profile directly:
        1. Add the content of `$HOME/export-esp.sh` to your shell’s profile: `cat $HOME/export-esp.sh >> [path to profile]`, for example, `cat $HOME/export-esp.sh >> ~/.bashrc`.
        2. Refresh the configuration by restarting the terminal session or by running `source [path to profile]`, for example, `source ~/.bashrc`.

        This approach **doesn't require any sourcing**. The `export-esp.sh` script will be sourced automatically in every shell.

[espup-github]: https://github.com/esp-rs/espup
[release-binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

#### What `espup` Installs

To enable support for Espressif targets, `espup` installs:

- [Espressif Rust fork][esp-rs/rust] with support for Espressif targets
- `stable` toolchain with support for RISC-V targets
- LLVM [fork][llvm-github-fork] with support for Xtensa targets
- [GCC toolchain][gcc-toolchain-github-fork] that links the final binary

The forked compiler can coexist with the standard Rust compiler, allowing both to be installed on your system. The forked compiler is invoked when using any of the available [overriding methods][rustup-overrides].

> ⚠️ **Note**: We are actively working to upstream our forks. Below is the current status:
> 1. LLVM fork: We've made significant progress recently. For details, refer to the [tracking issue][llvm-github-fork-upstream issue].
> 2. Rust compiler fork: We've submitted all feasible Xtensa patches. Further progress depends on these changes being upstreamed into LLVM.

[esp-rs/rust]: https://github.com/esp-rs/rust
[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-overrides]: https://rust-lang.github.io/rustup/overrides.html
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4

#### Other Installation Methods for Xtensa Targets

- Using [`rust-build`][rust-build] installation scripts. This was the recommended in the past, but now the installation scripts are feature frozen and all new features will only be included in `espup`. See the repository README for instructions.
- Building the Rust compiler with Xtensa support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It isn't recommended unless there is a major reason to go for this approach. Here is the repository to build it from source: [`esp-rs/rust` repository][esp-rs-rust].

[rust-build]: https://github.com/esp-rs/rust-build#download-installer-in-bash
[esp-rs-rust]: https://github.com/esp-rs/rust
