
# OpenOCD

Similar to [probe-rs][probe-rs], OpenOCD does not have support for the `Xtensa` architecture. However, Espressif does maintain a fork of OpenOCD under [espressif/openocd-esp32][espressif-openocd-esp32] which has support for Espressif's chips.

Instructions on how to install `openocd-esp32` for your platform can be found in [the Espressif documentation][espressif-documentation].

[probe-rs]: ./probe-rs.md
[espressif-openocd-esp32]: https://github.com/espressif/openocd-esp32
[espressif-documentation]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/index.html#setup-of-openocd

## Setup for Espressif Chips

<!-- how to choose interface & chip -->

Once installed, it's as simple as running `openocd` with the correct scripts. For chips with the built-in USB JTAG, there is normally a config that will work out of the box, for example on the ESP32-C3:

```shell
openocd -f board/esp32c3-builtin.cfg
```

For other configurations it may require specifying the chip and the interface, for example, ESP32 with a J-Link:

```shell
openocd -f interface/jlink.cfg -f target/esp32.cfg
```
