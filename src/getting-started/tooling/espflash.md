# `espflash`

[`espflash`][espflash] is a serial flasher utility designed for Espressif SoCs and modules. It natively supports all chips compatible with `esp-hal`.

## Installation

To install [`espflash`][espflash], execute the following command:

```shell
cargo install espflash --locked
```

Alternatively, you can use [`cargo-binstall`][cargo-binstall] to download pre-compiled artifacts from the [releases] and use them instead:

```bash
cargo binstall espflash
```

> [!NOTE]
> The default baudrate used by [`espflash`][espflash] is 115200 - you might want to increase it to make flashing faster.
> The easiest way to do that is setting the `ESPFLASH_BAUD` environment variable.

[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall
[releases]: https://github.com/esp-rs/espflash/releases
[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash/
