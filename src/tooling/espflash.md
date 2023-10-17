# `espflash`

`espflash` is a serial flasher utility, based on [esptool.py][esptool], for Espressif SoCs and modules.

The [`espflash`][espflash] repository contains two crates, `cargo-espflash` and `espflash`. For more information on these crates, see the respective sections below.

[esptool]: https://github.com/espressif/esptool
[espflash]: https://github.com/esp-rs/espflash

> ⚠️ **Note**: The `espflash` and `cargo-espflash` commands shown below, assume that version `2.0` or greater is used.

## `cargo-espflash`

Provides a subcommand for `cargo` that handles cross-compilation and flashing.

To install `cargo-espflash`, ensure that you have the [necessary dependencies][cargo-espflash-dependencies] installed, and then execute the following command:

```shell
cargo install cargo-espflash
```

This command must be run within a Cargo project, ie. a directory containing a `Cargo.toml` file. For example, to build an example named 'blinky', flash the resulting binary to a device, and then subsequently start a serial monitor:

```shell
cargo espflash flash --example=blinky --monitor
```

For more information, please see the [`cargo-espflash`][cargo-espflash] README.

[cargo-espflash]: https://github.com/esp-rs/espflash/blob/master/cargo-espflash/README.md
[cargo-espflash-dependencies]: https://github.com/esp-rs/espflash/blob/main/cargo-espflash/README.md#installation

## `espflash`

Provides a standalone command-line application that flashes an ELF file to a device.

To install `espflash`, ensure that you have the [necessary dependencies][espflash-dependencies] installed, and then execute the following command:

```shell
cargo install espflash
```

Assuming you have built an ELF binary by other means already, `espflash` can be used to download it to your device and monitor the serial port. For example, if you have built the `getting-started/blinky` example from [ESP-IDF][esp-idf] using `idf.py`, you might run something like:

```shell
espflash flash build/blinky --monitor
```

For more information, please see the [`espflash` README][espflash-readme].

`espflash` can be used as a Cargo runner by adding the following to your project's `.cargo/config.toml` file:
```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "espflash flash --monitor"
```
With this configuration, you can flash and monitor your application using `cargo run`.

[esp-idf]: https://github.com/espressif/esp-idf
[espflash-readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md
[espflash-dependencies]:https://github.com/esp-rs/espflash/blob/main/espflash/README.md#installation
