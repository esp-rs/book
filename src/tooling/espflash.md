# espflash

A serial flasher utility for ESP devices. Supports flashing _ESP32_, _ESP32-C2_, _ESP32-C3_, _ESP32-S2_, _ESP32-S3_, and _ESP8266_.

The [esp-rs/espflash] repository contains two crates, `cargo-espflash` and `espflash`. You can find more information on both of these in their respective sections below.

> #### A note on `espflash` and `cargo-espflash`.
>
> The `espflash` and `cargo-espflash` commands shown below, assume that version `2.0` or
> greater is installed.

[esp-rs/espflash]: https://github.com/esp-rs/espflash

## cargo-espflash

Provides a subcommand for `cargo` that handles cross-compilation and flashing. Note that this requires the unstable `build-std` cargo feature; for more information on this please refer to [the cargo documentation].

See [Installation section of cargo-espflash README] for details on how to install it.

This command must be run within a Cargo project, ie.) a directory containing a `Cargo.toml` file. For example, to build an example named 'blinky' in `release` mode, flash the resulting binary to a device, and then subsequently start a serial monitor:

```bash
cargo espflash flash --example=blinky --release --monitor
```

For more information about usage, please see [Usage section of cargo-espflash README].

[the cargo documentation]: https://doc.rust-lang.org/cargo/reference/unstable.html#build-std
[Installation section of cargo-espflash README]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash#installation
[Usage section of cargo-espflash README]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash#usage

## espflash

Provides a standalone command-line application that flashes an ELF file to a device.

See [Installation section of espflash README] for details on how to install it.

Assuming you have built an ELF binary by other means already, `espflash` can be used to download it to your device and monitor the serial port. For example, if you have built the `getting-started/blinky` example from [esp-idf] using `idf.py` you might run something like:

```bash
espflash flash build/blinky --monitor
```
See [Usage section of espflash README] for more details and other commands.

[Installation section of espflash README]: https://github.com/esp-rs/espflash/tree/main/espflash#installation
[esp-idf]: https://github.com/espressif/esp-idf
[espflash readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md
[Usage section of espflash README]: https://github.com/esp-rs/espflash/tree/main/espflash#usage
