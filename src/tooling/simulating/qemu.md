# QEMU

Espressif maintains a fork of QEMU in [espressif/QEMU][espressif-qemu] with the necessary patches to make it work on Espressif chips.
See the [ESP-specific instructions for running QEMU][esp-qemu-doc] for instructions on how to build QEMU and emulate projects with it.

Once you have built QEMU, you should have the `qemu-system-xtensa` file.

[espressif-qemu]: https://github.com/espressif/qemu
[esp-qemu-doc]: https://github.com/espressif/esp-toolchain-docs/tree/main/qemu/esp32#overview

## Running Your Project Using QEMU

> ⚠️ **Note**: Only ESP32 is currently supported, so make sure you are compiling for `xtensa-esp32-espidf` target.

For running our project in QEMU, we need a firmware/image with bootloader and partition table merged in it.
We can use [`cargo-espflash`][cargo-espflash] to generate it:

```shell
cargo espflash save-image --chip esp32 --merge <OUTFILE> --release
```

If you prefer to use [`espflash`][espflash], you can achieve the same result by building the project first and then generating image:

```shell
cargo build --release
espflash save-image --chip ESP32 --merge target/xtensa-esp32-espidf/release/<NAME> <OUTFILE>
```

Now, run the image in QEMU:

```shell
/path/to/qemu-system-xtensa -nographic -machine esp32 -drive file=<OUTFILE>,if=mtd,format=raw
```

[cargo-espflash]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash
[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
