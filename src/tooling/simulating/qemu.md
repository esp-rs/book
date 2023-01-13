# QEMU

Espressif maintains a fork of QEMU in [espressif/QEMU] with the necessary patches to make it work on Espressif chips.
See the [QEMU wiki] for instructions on how to build QEMU and emulate projects with it.

Once you have built QEMU, you should have `qemu-system-xtensa`.

[espressif/QEMU]: https://github.com/espressif/qemu
[QEMU wiki]: https://github.com/espressif/qemu/wiki

## Running our project using QEMU

> *NOTE*: Only ESP32 is currently supported, so make sure you are compiling for `xtensa-esp32-espidf` target.

For running our project in QEMU, we need a firmware/image with bootloader and partition table merged in it.
We can use [`cargo-espflash`] to generate it:

[`cargo-espflash`]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash

```bash
cargo espflash save-image --merge ESP32 <OUTFILE> --release
```

> If you prefer to use [`espflash`], you can achieve the same result by building the project first and then generating image. Following an excerpt from `espflash save-image --help`.
>```
>USAGE:
>    espflash save-image [OPTIONS] <CHIP> <IMAGE> <FILE>
>
>ARGS:
>    <CHIP>     Chip to create an image for [possible values: ESP32, ESP32-C2, ESP32-C3,
>               ESP32-S2, ESP32-S3, ESP8266]
>    <IMAGE>    ELF image to flash
>    <FILE>     File name to save the generated image to
>OPTIONS:
>    [..]
>    -M, --merge
>            Boolean flag, if set, bootloader, partition table and application binaries will be
>            merged into single binary
>```
> For example:
>```bash 
> cargo build --release
> espflash save-image --merge ESP32 target/xtensa-esp32-espidf/release/<NAME> <OUTFILE>
>```

[`espflash`]: https://github.com/esp-rs/espflash/tree/main/espflash

Now, run the image in QEMU:
```sh
/path/to/qemu-system-xtensa -nographic -machine esp32 -drive file=<OUTFILE>,if=mtd,format=raw
```

