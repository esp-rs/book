# espflash

A serial flasher utility for ESP devices. Supports flashing _ESP32_, _ESP32-C2_, _ESP32-C3_, _ESP32-S2_, _ESP32-S3_, and _ESP8266_.

The [esp-rs/espflash] repository contains two crates, `cargo-espflash` and `espflash`. You can find more information on both of these in their respective sections below.

[esp-rs/espflash]: https://github.com/esp-rs/espflash

## cargo-espflash

Provides a subcommand for `cargo` which handles cross-compilation and flashing. Note that this requires the unstable `build-std` cargo feature; for more information on this please refer to [the cargo documentation].

To install:

```bash
cargo install cargo-espflash
```

This command must be run within a Cargo project, ie.) a directory containing a `Cargo.toml` file. For example, to build an example named 'blinky' in `release` mode, flash the resulting binary to a device, and then subsequently start a serial monitor:

```bash
cargo espflash --example=blinky --release --monitor
```

For more information please see the [cargo-espflash README].

[the cargo documentation]: https://doc.rust-lang.org/cargo/reference/unstable.html#build-std
[cargo-espflash readme]: https://github.com/esp-rs/espflash/blob/master/cargo-espflash/README.md

## espflash

Provides a standalone command-line application that flashes an ELF file to a device.

To install:

```bash
cargo install espflash
```

Assuming you have built an ELF binary by other means already, `espflash` can be used to download it to your device. For example, if you have built the `getting-started/blinky` example from [esp-idf] using `idf.py` you might run something like:

```bash
espflash build/blinky
```

For more information please see the [espflash README].

[esp-idf]: https://github.com/espressif/esp-idf
[espflash readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md
