# espflash

A serial flasher utility for ESP devices. Supports flashing _ESP32_, _ESP32-C2_, _ESP32-C3_, _ESP32-C6_,, _ESP32-H2_ _ESP32-S2_, _ESP32-S3_, and _ESP8266_.

The [esp-rs/espflash] repository contains two crates, `cargo-espflash` and `espflash`. You can find more information on both of these in their respective sections below and in their corresponding README.

> #### A note on `espflash` and `cargo-espflash`.
>
> The `espflash` and `cargo-espflash` commands shown below, assume that version `2.0` or greater.

[esp-rs/espflash]: https://github.com/esp-rs/espflash

## cargo-espflash

Provides a subcommand for `cargo` that handles cross-compilation and flashing.

To install:

```bash
cargo install cargo-espflash
```

This command must be run within a Cargo project, ie. a directory containing a `Cargo.toml` file. For example, to build an example named 'blinky', flash the resulting binary to a device, and then subsequently start a serial monitor:

```bash
cargo espflash flash --example=blinky --monitor
```

For more information please see to the [cargo-espflash README].

[cargo-espflash readme]: https://github.com/esp-rs/espflash/blob/master/cargo-espflash/README.md

## espflash

Provides a standalone command-line application that flashes an ELF file to a device.

To install:

```bash
cargo install espflash
```

Assuming you have built an ELF binary by other means already, `espflash` can be used to download it to your device and monitor the serial port. For example, if you have built the `getting-started/blinky` example from [esp-idf] using `idf.py` you might run something like:

```bash
espflash flash build/blinky --monitor
```

For more information please see to the [espflash README].


`espflash` can be used as a Cargo runner by adding the following to your project's `.cargo/config.toml` file:
```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "espflash flash --monitor"
```
With this configuration you can flash and monitor you application using `cargo run`.

[esp-idf]: https://github.com/espressif/esp-idf
[espflash readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md
