# `espflash`

`espflash` is a serial flasher utility, based on [esptool.py][esptool], for Espressif SoCs and modules.

The [`espflash`][espflash] repository contains two crates, `cargo-espflash` and `espflash`. For more information on these crates, see the respective sections below.

[esptool]: https://github.com/espressif/esptool
[espflash]: https://github.com/esp-rs/espflash

## `espflash`

Provides a standalone command-line application for flashing, monitoring, erasing, and reading memory, along with many other features. For a complete list of available commands, refer to the [Usage section][espflash-usage] of the README.

`espflash` can also be [used as a library][espflash-library], supports a [configuration file][espflash-config], works within [WSL2][espflash-wsl2], and includes support for [`defmt`][espflash-formats].

[espflash-usage]: https://github.com/esp-rs/espflash/tree/main/espflash#usage
[espflash-config]: https://github.com/esp-rs/espflash/tree/main/espflash#configuration-file
[espflash-wsl2]: https://github.com/esp-rs/espflash/tree/main/espflash#windows-subsystem-for-linux
[espflash-library]: https://github.com/esp-rs/espflash/tree/main/espflash#using-espflash-as-a-library
[espflash-formats]:https://github.com/esp-rs/espflash/tree/main/espflash#logging-format

### Installation

To install `espflash`, execute the following command:

```shell
cargo install espflash --locked
```

### Flashing and Monitoring an Application

Assuming you have built an ELF binary by other means already, `espflash` can be used to download it to your device and monitor the serial port with:

```shell
espflash flash --monitor your-elf
```
For more information, please see the [`espflash` README][espflash-readme].

[espflash-readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md

#### Using `espflash` as a Cargo Runner

`espflash` can be used as a Cargo runner by adding the following to your project's `.cargo/config.toml` file:

```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "espflash flash --monitor"
```

With this configuration, you can flash and monitor your application using `cargo run`.

This is automatically done when the project is generated via [`esp-generate`][esp-generate].

[esp-generate]: esp-generate.md

## `cargo-espflash`

Provides a subcommand for `cargo` that handles cross-compilation and interaction with the target.

`cargo-espflash` supports a [configuration file][cargo-espflash-config], works within [WSL2][cargo-espflash-wsl2], and includes support for [`defmt`][cargo-espflash-formats].

[cargo-espflash-config]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash#configuration-file
[cargo-espflash-wsl2]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash#windows-subsystem-for-linux
[cargo-espflash-formats]:https://github.com/esp-rs/espflash/tree/main/cargo-espflash#logging-format

### Installation

To install `cargo-espflash`, execute the following command:

```shell
cargo install cargo-espflash --locked
```

> ⚠️ **Note**: in Unix systems, we use the [`vendored-openssl` Cargo feature][vendored-ssl] which may require additional tools such as `perl` and `make`. To disable this feature, use:
> ```shell
> OPENSSL_NO_VENDOR=1 cargo install cargo-espflash
> ```

### Building, Flashing and Monitoring an Application

This command must be run within a Cargo project, ie. a directory containing a `Cargo.toml` file. For example, to build an application named 'blinky', flash the resulting binary to a device, and then subsequently start a serial monitor:

```shell
cargo espflash flash --monitor
```

For more information, please see the [`cargo-espflash`][cargo-espflash] README.

[cargo-espflash]: https://github.com/esp-rs/espflash/blob/master/cargo-espflash/README.md
[vendored-ssl]: https://github.com/rust-lang/cargo#compiling-from-source

