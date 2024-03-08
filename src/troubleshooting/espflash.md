# `espflash`

## `rtc_clk_init: Possibly invalid CONFIG_XTAL_FREQ setting`

This issue is reported by users using an ESP32 module with a 26MHz crystal oscillator. The root
cause of the issue is that the default bootloader flashed by espflash expects a 40MHz crystal.

### If you are building an `esp-idf-sys` based project

Make sure your [`sdkconfig`](https://github.com/esp-rs/esp-idf-sys/blob/master/BUILD-OPTIONS.md#sdkconfig)
is set up properly to use a 26MHz crystal. It needs to contain the following configuration option:

```
CONFIG_XTAL_FREQ_26=y
```

You should also prefer using `cargo-espflash` over `espflash`. `cargo-espflash` integrates with your
project and it will flash the bootloader that is built next to your project instead of the default
one.

If you want to use `espflash`, you need to specify an appropriate bootloader image using
`--bootloader`. You can find the bootloader in `target/<your MCU's target folder>/<debug or release depending on your build>/build/esp-idf-sys-*/build/bootloader/bootloader.bin`
