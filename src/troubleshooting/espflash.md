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

### If you are building an `esp-hal` based project

Make sure your HAL ([ESP32](https://docs.rs/esp32-hal/latest/esp32_hal/), [ESP32-C2](https://docs.rs/esp32c2-hal/latest/esp32c2_hal/))
is configured to the correct crystal frequency. To do this, you must disable the default features
and enable `xtal-26mhz` (besides the other default features).

When flashing, you need to specify an appropriate bootloader image using `--bootloader`. Currently,
you will need to build this bootloader using an `esp-idf` based project (Rust or C based should work
equally, we recommend a project set up with [esp-idf-template](https://github.com/esp-rs/esp-idf-template)).
