# `RISC-V` and `Xtensa` Targets

[`espup`][espup-github] is a tool that simplifies installing and maintaining the components required to develop Rust applications for the `Xtensa` and `RISC-V` architectures.

### 1. Install `espup`

To install `espup`, run:
```shell
cargo install espup
```

You can also directly download pre-compiled [release binaries][release-binaries] or use [`cargo-binstall`][cargo-binstall].

[espup-github]: https://github.com/esp-rs/espup
[release-binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

### 2. Install Necessary Toolchains

Install all the necessary tools to develop Rust applications for all supported Espressif targets by running:
```shell
espup install
```

> ⚠️ **Note**: `std` applications require installing additional software covered in [`std` Development Requirements][rust-esp-book-std-requirements]

[rust-esp-book-std-requirements]: ./std-requirements.md

### 3. Set Up the Environment Variables
`espup` will create an export file that contains some environment variables required to build projects.

On Windows (`%USERPROFILE%\export-esp.ps1`)
  - There is **no need** to execute the file for Windows users. It is only created to show the modified environment variables.

On Unix-based systems (`$HOME/export-esp.sh`). There are different ways of sourcing the file:
- Source this file in every terminal:
   1. Source the export file: `. $HOME/export-esp.sh`

   This approach requires running the command in every new shell.
- Create an alias for executing the `export-esp.sh`:
   1. Copy and paste the following command to your shell’s profile (`.profile`, `.bashrc`, `.zprofile`, etc.): `alias get_esprs='. $HOME/export-esp.sh'`
   2. Refresh the configuration by restarting the terminal session or by running `source [path to profile]`, for example, `source ~/.bashrc`.

   This approach requires running the alias in every new shell.
- Add the environment variables to your shell profile directly:
   1. Add the content of `$HOME/export-esp.sh` to your shell’s profile: `cat $HOME/export-esp.sh >> [path to profile]`, for example, `cat $HOME/export-esp.sh >> ~/.bashrc`.
   2. Refresh the configuration by restarting the terminal session or by running `source [path to profile]`, for example, `source ~/.bashrc`.

   This approach **doesn't** require any sourcing. The `export-esp.sh` script will be sourced automatically in every shell.

### What `espup` Installs

To enable support for Espressif targets, `espup` installs the following tools:

- Espressif Rust fork with support for Espressif targets
- `nightly` toolchain with support for `RISC-V` targets
- `LLVM` [fork][llvm-github-fork] with support for `Xtensa` targets
- [GCC toolchain][gcc-toolchain-github-fork] that links the final binary

The forked compiler can coexist with the standard Rust compiler, allowing both to be installed on your system. The forked compiler is invoked when using any of the available [overriding methods][rustup-overrides].

> ⚠️ **Note**: We are making efforts to upstream our forks
> 1. Changes in `LLVM` fork. Already in progress, see the status in this [tracking issue][llvm-github-fork-upstream issue].
> 2. Rust compiler forks. If `LLVM` changes are accepted, we will proceed with the Rust compiler changes.

If you run into an error, please, check the [Troubleshooting][troubleshooting] chapter.

[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-overrides]: https://rust-lang.github.io/rustup/overrides.html
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4
[troubleshooting]: ../troubleshooting/index.md

### Other Installation Methods for `Xtensa` Targets

- Using [`rust-build`][rust-build] installation scripts. This was the recommended way in the past, but now the installation scripts are feature frozen, and all new features will only be included in `espup`. See the repository README for instructions.
- Building the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It isn't recommended unless there is a major reason to go for this approach. Here is the repository to build it from source: [`esp-rs/rust` repository][esp-rs-rust].

[rust-build]: https://github.com/esp-rs/rust-build#download-installer-in-bash
[esp-rs-rust]: https://github.com/esp-rs/rust
