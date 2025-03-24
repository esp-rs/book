# Frequently Asked Questions

This chapter addresses common questions and challenges that developers encounter when working with Rust on ESP chips. Whether you're setting up your development environment, optimizing your code, or looking to simulate your projects, you'll find practical solutions and best practices here.

## Editor/IDE

When using `esp-generate`, you can automatically configure recommended settings and extensions for both VS Code and Helix editors.

### Visual Studio Code

VS Code provides great support for Rust ESP development with the right extensions and settings.

#### Recommended Extensions

- [`rust-analyzer`](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) - Provides intelligent code completion, go to definition, and other IDE features
  - Requires the `rust-analyzer` component: `rustup component add rust-analyzer`
- [`even-better-toml`](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) - TOML file support with syntax highlighting and validation
- [`dependi`](https://marketplace.visualstudio.com/items?itemName=fill-labs.dependi) - Dependency management and updates

#### Recommended Settings

##### Xtensa Targets

```json
{
  "rust-analyzer.cargo.allTargets": false,
  "rust-analyzer.server.extraEnv": {
    "RUSTUP_TOOLCHAIN": "stable"
  },
  "rust-analyzer.check.extraEnv": {
    "RUSTUP_TOOLCHAIN": "esp"
  },
  "rust-analyzer.cargo.extraEnv": {
    "RUSTUP_TOOLCHAIN": "esp"
  },
}
```

##### RISC-V Targets

```json
{
  "rust-analyzer.cargo.allTargets": false,
  "rust-analyzer.cargo.target": "riscv32imac-unknown-none-elf",
}
```

### Helix

[Helix][helix] is a modal text editor, inspired in Kakoune/Neovim, that works well for ESP Rust development

[helix]: https://helix-editor.com/

#### Recommended Settings

##### Xtensa Targets

```toml
[[language]]
name = "rust"

[language-server.rust-analyzer]
environment.RUSTUP_TOOLCHAIN = "stable"

[language-server.rust-analyzer.config]
check.allTargets = false
cargo.target = "xtensa-esp32-none-elf"
check.extraEnv.RUSTUP_TOOLCHAIN = "esp"
cargo.extraEnv.RUSTUP_TOOLCHAIN = "esp"
```

##### RISC-V Targets

```toml
[[language]]
name = "rust"

[language-server.rust-analyzer.config]
check.allTargets = false
cargo.target = "riscv32imac-unknown-none-elf"
```

## Size and Memory Optimizations

### Optimizing Binary Size

- Cargo provides some default profiles; we recommend using the [`release` profile][release-profile] as it optimizes and removes debug symbols.
- Cargo allows different [profile settings][profile-settings-cargo], which can make a difference in the resulting size of the artifact.
  - See [Optimizations: the speed size tradeoff][embedded-book-tradeoffs] of The Embedded Rust Book.
- Be careful when using external dependencies, as they can increase the size of your resulting artifact
- Filter log messages if they are not going to be useful or read.

[embedded-book-tradeoffs]: https://docs.rust-embedded.org/book/unsorted/speed-vs-size.html
[release-profile]: https://doc.rust-lang.org/cargo/reference/profiles.html#release
[profile-settings-cargo]: https://doc.rust-lang.org/cargo/reference/profiles.html#profile-settings

### Reducing Memory Usage

- Minimize heap allocations
- Optimize Struct Layouts to improve the padding

## Using Crates from Git

### Using a Crate Version from a Repository

If for some reason you don't want to use a published version, you can use a Git repository directly. To specify a crate from a Git repository and a specific branch:
```toml
[dependencies]
esp-hal = { git = "https://github.com/esp-rs/esp-hal.git", package="esp-hal", branch = "main" }
```

You can also specify a specific commit or tag:
```toml
[dependencies]
esp-hal = { git = "https://github.com/esp-rs/esp-hal.git", package="esp-hal", rev = "abc123" }
```
 See [Specifying dependencies from git repositories][cargo-dependencies-git] section of The Cargo Book for more information.

[cargo-dependencies-git]: https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#specifying-dependencies-from-git-repositories

### Applying Patches to Dependencies

If you need to override a dependency with a local or different version, use the `[patch]` section in `Cargo.toml`:
```toml
[patch.crates-io]
esp-hal = { git = "https://github.com/esp-rs/esp-hal.git", branch = "main" }
```

For more information, see [`[patch]` section][cargo-patch] of The Cargo Book.

[cargo-patch]: https://doc.rust-lang.org/cargo/reference/config.html?highlight=patches#patch

## Simulating Projects

Simulating projects can be handy. It allows users to test projects using CI, try projects without having hardware available, and many other scenarios.

At the moment, there are a few ways of simulating Rust projects on Espressif chips. Every way has some limitations, but it's quickly evolving and getting better every day.

In this chapter, we will discuss currently available simulation tools.

Refer to the table below to see which chip is supported in every simulating method:

|              | **[Wokwi][wokwi]** | **QEMU** |
| :----------: | :----------------: | :------: |
|  **ESP32**   |         ✅          |    ✅     |
| **ESP32-C2** |         ❌          |    ❌     |
| **ESP32-C3** |         ✅          |    ✅     |
| **ESP32-C6** |         ✅          |    ❌     |
| **ESP32-H2** |         ✅          |    ❌     |
| **ESP32-S2** |         ✅          |    ❌     |
| **ESP32-S3** |         ✅          |    ✅     |

[wokwi]: https://docs.wokwi.com/guides/esp32#simulation-features

### Wokwi

[Wokwi][wokwi] is an online simulator that supports simulating Rust projects (`no_std`) in Espressif Chips.
See [wokwi.com/rust][wokwi-rust] for a list of examples and a way to start new projects.

Wokwi offers Wi-Fi simulation, Virtual Logic Analyzer, and [GDB debugging][gdb-debugging] among many other features; see
[Wokwi documentation][wokwi-documentation] for more details. For ESP chips, there is a table of [simulation features][wokwi-simulation-features] that are currently supported.

[wokwi]: https://wokwi.com/
[wokwi-rust]: https://wokwi.com/rust
[gdb-debugging]: https://docs.wokwi.com/gdb-debugging
[wokwi-documentation]: https://docs.wokwi.com/
[wokwi-simulation-features]: https://docs.wokwi.com/guides/esp32#simulation-features

#### Using Wokwi for VS Code extension

Wokwi offers a VS Code extension that allows you to simulate a project directly in the code editor by only adding a few files.
For more information, see [Wokwi documentation][wokwi-vscode].
You can also debug your code using the VS Code debugger; see [Debugging your code][wokwi-debugging].

When using any of the [templates][templates] and not using the default values, there is a prompt (`Configure project to support Wokwi simulation with Wokwi VS Code extension?`) that generates the required files to use Wokwi VS Code extension.


[wokwi-vscode]: https://docs.wokwi.com/vscode/getting-started
[wokwi-debugging]: https://docs.wokwi.com/vscode/debugging
[templates]: ./../../writing-your-own-application/generate-project/index.md

#### Using `wokwi-server`

[`wokwi-server`][wokwi-server] is a CLI tool for launching a Wokwi simulation of your project. I.e., it allows you
to build a project on your machine, or in a container, and simulate the resulting binary.

[`wokwi-server`][wokwi-server] also allows simulating your resulting binary on other Wokwi projects, with more hardware parts other than the chip itself. See the corresponding [section of the `wokwi-server`][wokwi-server-custom] README for detailed instructions.

[wokwi-server]: https://github.com/MabezDev/wokwi-server
[wokwi-server-custom]: https://github.com/MabezDev/wokwi-server#simulating-your-binary-on-a-custom-wokwi-project

#### Custom Chips

Wokwi allows generating custom chips that let you program the behavior of a component not supported in Wokwi. For more details, see the official [Wokwi documentation][wokwi-custom-chip].

Custom chips can also be written in Rust! See [Wokwi Custom Chip API][rust-chip-api] for more information. For example, custom [inverter chip][custom-chip-example] in Rust.

[wokwi-custom-chip]: https://docs.wokwi.com/chips-api/getting-started
[rust-chip-api]: https://github.com/wokwi/wokwi_chip_ll
[custom-chip-example]: https://github.com/wokwi/rust_chip_inverter


### QEMU

Espressif maintains a fork of QEMU in [espressif/QEMU][espressif-qemu] with the necessary patches to make it work on Espressif chips.
See the [ESP-specific instructions for running QEMU][esp-qemu-doc] for instructions on how to build QEMU and emulate projects with it.

Once you have built QEMU, you should have the `qemu-system-xtensa` file.

[espressif-qemu]: https://github.com/espressif/qemu
[esp-qemu-doc]: https://github.com/espressif/esp-toolchain-docs/tree/main/qemu/esp32#overview

#### Running Your Project Using QEMU

> ⚠️ **Note**: Only ESP32 is currently supported, so make sure you are compiling for `xtensa-esp32-espidf` target.

For running our project in QEMU, we need a firmware/image with bootloader and partition table merged in it.
We can use [`cargo-espflash`][cargo-espflash] to generate it:

```shell
cargo espflash save-image --chip esp32 --merge <OUTFILE> --release
```

If you prefer to use [`espflash`][espflash], you can achieve the same result by building the project first and then generating image:

```shell
cargo build --release
espflash save-image --chip esp32 --merge target/xtensa-esp32-espidf/release/<NAME> <OUTFILE>
```

Now, run the image in QEMU:

```shell
/path/to/qemu-system-xtensa -nographic -machine esp32 -drive file=<OUTFILE>,if=mtd,format=raw -m 4M
```

[cargo-espflash]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash
[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
