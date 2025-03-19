# `esp-generate`

[`esp-generate`][esp-generate] is a template generation tool that assists users in creating a functional project with most of their desired configurations pre-applied.

## Instalaltion
To install `esp-generate`:

```shell
cargo install esp-generate --locked
```

You can also directly download pre-compiled [release binaries][release-binaries] or use [`cargo-binstall`][cargo-binstall].

[release-binaries]: https://github.com/esp-rs/esp-generate/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

## Generating a Project

To launch the TUI and select your configuration options, run:

```shell
esp-generate --chip esp32c3 your-project
```

Replace the chip and project name accordingly.

If you prefer not to use the TUI, you can specify options directly with the `-o/--options` flag. Additionally, using the `--headless` flag will prevent the TUI from launching:

```shell
esp-generate --chip esp32 -o alloc -o wifi --headless your-project
```

Adjust the options as needed for your project. See [Available Options][available-options] section of the README.

The tool will generate the following files:
- [`build.rs`][build.rs]
    - Sets the linker script arguments based on the template options.
- [`.cargo/config.toml`][config-toml]
    - The Cargo configuration
    - This defines a few options to correctly build the project
    - Contains the custom runner command for `espflash` or `probe-rs`. For example, `runner = "espflash flash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
- [`Cargo.toml`][cargo-toml]
    - The usual Cargo manifest declares some meta-data and dependencies of the project
- [`.gitignore`][gitignore]
    - Tells `git` which folders and files to ignore
- [`rust-toolchain.toml`][rust-toolchain-toml]
    - Defines which Rust toolchain to use
      - The toolchain will be `stable`, `nightly` or `esp` depending on your target
- `src/bin/main.rs`
    - The main source file of the newly created project
- `src/lib.rs`
    - This tells the Rust compiler that this code doesn't use `libstd`

Some of the options may add some other files to your generated project:
- When using the `ci` option:
  - `.github/workflows/rust_ci.yml`:
    - Defines a GitHub Actions workflow for continuous integration (CI), ensuring that the project builds
- When using the `devcontainer` option:
  - `.devcontainer/Dockerfile`
    - Specifies the Docker container environment used for development, including necessary dependencies and configurations
  - `.devcontainer/devcontainer.json`
    - Configures the `Dev Container` environment for VS Code, defining settings, extensions, and additional configurations
  - `.dockerignore`:
    - Lists files and directories to exclude from the Docker image
- When using the `wokwi` option:
  - `wokwi.toml`
    - [Configuration file][wokwi-toml] that tells [Wokwi][wokwi] how to run your project in VS Code
  - `diagram.json`
    - [Diagram file][diagram-file] that describes the circuit
- When using the `helix` option:
  - `.helix/languages.toml`
    - Configures language-specific settings for the Helix text editor
- When using the `vscode` option:
  - `.vscode/extensions.json`
    - Lists recommended VS Code extensions
  - `.vscode/settings.json`
    - Contains reccomended VS Code settings

[esp-generate]: https://github.com/esp-rs/esp-generate
[available-options]: https://github.com/esp-rs/esp-generate?tab=readme-ov-file#available-options
[build.rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[cargo-toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[gitignore]: https://git-scm.com/docs/gitignore
[config-toml]: https://doc.rust-lang.org/cargo/reference/config.html
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[wokwi-toml]: https://docs.wokwi.com/vscode/project-config
[wokwi]: https://wokwi.com/
[diagram-file]: https://docs.wokwi.com/diagram-format

## Running the Code

Building and running the code is as easy as:

```shell
cargo run --release
```

This builds your application, flashes it to the target device and will open a serial monitor.
<!-- TODO: Shall we explain the fron and backend options here? -->


