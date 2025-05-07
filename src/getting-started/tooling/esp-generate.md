# `esp-generate`

[`esp-generate`][esp-generate] is a project generation tool that assists users in creating a functional project with most of their desired configurations pre-applied.

## Installation
To install `esp-generate`:

```shell
cargo install esp-generate --locked
```

You can also directly download pre-compiled [release binaries][release-binaries] or use [`cargo-binstall`][cargo-binstall].

[release-binaries]: https://github.com/esp-rs/esp-generate/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

## Generating a Project

To launch an interactive tool at your terminal to select your configuration options, run:

```shell
esp-generate --chip esp32c3 your-project-name
```

Replace the chip and project name accordingly.

If you prefer to specify the options in a non-interactive way, you can specify options directly with the `-o/--options` flag. Additionally, using the `--headless` flag will prevent the TUI (Terminal User Interface) from launching:

```shell
esp-generate --chip esp32 -o alloc -o wifi --headless your-project-name
```

Adjust the options as needed for your project. See [Available Options][available-options] section of the README.

[esp-generate]: https://github.com/esp-rs/esp-generate
[available-options]: https://github.com/esp-rs/esp-generate?tab=readme-ov-file#available-options

## Running the Code

Building and running the code is as easy as:

```shell
cargo run --release
```

This builds your application, flashes it to the target device and opens a serial monitor. The command uses the runner configuration in `.cargo/config.toml` that was automatically set up by `esp-generate`.

<!-- TODO: Shall we explain the fron and backend options here? -->


