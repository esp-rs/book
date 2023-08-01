# `probe-rs`

The [`probe-rs`][probe-rs] project is a set of tools to interact with embedded MCU's using various debug probes. It is similar to [OpenOCD][openocd], [pyOCD][pyocd], [Segger tools][segger-tools], etc. There is support for `ARM` & `RISC-V` architectures along with a collection of tools, including but not limited to:

- Debugger
  - GDB support.
  - CLI for interactive debugging.
  - VS Code extension.
- Real Time Transfer (RTT)
  - Similar to app_trace component of IDF.
- Flashing algorithms

More info about probe-rs & how to set up a project can be found on the [probe-rs] website.

[probe-rs]: https://probe.rs/
[openocd]: https://openocd.org/
[pyocd]: https://pyocd.io/
[segger-tools]: https://www.segger.com/

## `USB-JTAG-SERIAL` Peripheral for ESP32-C3

Starting from `probe-rs` v0.12, it is possible to flash and debug the ESP32-C3 with the built-in `USB-JTAG-SERIAL` peripheral, no need for any external hardware debugger. More info on configuring the interface can be found in the [official documentation][official-documentation].

[official-documentation]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/configure-builtin-jtag.html

## Support for Espressif Chips

`probe-rs` currently only supports `ARM` & `RISC-V`, therefore this limits the number of Espressif chips that can be used at the moment.

|   Chip   | Flashing | Debugging |
| :------: | :------: | :-------: |
| ESP32-C3 |    ✅     |     ⚠️     |

> ⚠️ **Note**: _Items marked with ⚠️ are currently work in progress, usable but expect bugs._

## Permissions - Linux

On Linux, you may run into permission issues trying to interact with Espressif probes. Installing the following `udev` rules and reloading should fix that issue.

```text
# Espressif dev kit FTDI
ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6010", MODE="660", GROUP="plugdev", TAG+="uaccess"

# Espressif USB JTAG/serial debug unit
ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1001", MODE="660", GROUP="plugdev", TAG+="uaccess"

# Espressif USB Bridge
ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1002", MODE="660", GROUP="plugdev", TAG+="uaccess"
```

<!-- TODO: when probe-rs can actually debug at least a C3 with decent back traces etc, add a section here with an example config: see https://github.com/probe-rs/probe-rs/issues/877 -->
