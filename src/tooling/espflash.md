# espflash

A serial flasher utility for ESP devices. Supports flashing _ESP32_, _ESP32-C3_, _ESP32-S2_, and _ESP8266_.

The [esp-rs/espflash] repository contains two crates, `cargo-espflash` and `espflash`. You can find more information on both of these in their respective sections below.

[esp-rs/espflash]: https://github.com/esp-rs/espflash

## cargo-espflash

Provides a subcommand for `cargo` which handles cross-compilation and flashing. Note that this requires the unstable `build-std` cargo feature; for more information on this please refer to [the cargo documentation].

To install:

```bash
$ cargo install cargo-espflash
```

This command must be run within a Cargo project, ie.) a directory containing a `Cargo.toml` file. For example, to build an example named 'blinky' in `release` mode, flash the resulting binary to a device, and then subsequently start a serial monitor:

```bash
$ cargo espflash --release --monitor --example=blinky /dev/ttyUSB0
```

[the cargo documentation]: https://doc.rust-lang.org/cargo/reference/unstable.html#build-std

## espflash

Provides a standalone command-line application which flashes an ELF file to a device.

To install:

```bash
$ cargo install espflash
```
