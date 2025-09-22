# `probe-rs`

The [`probe-rs`][probe-rs] project offers a suite of tools for interacting with embedded microcontrollers using various debug probes, with robust support for [Espressif chips][chips].

Besides flashing and monitoring, it also provides robust debugging capabilities.

If you're unsure whether you need it right now, you can skip the installation and come back to it later.

Espressif devices equipped with the  `USB-JTAG-SERIAL` peripheral can use `probe-rs` without any external hardware. For devices lacking this peripheral, you'll need an external programmer like the [ESP-Prog][esp-prog].

> ⚠️ **Note**: `USB-JTAG-SERIAL` peripheral is available in ESP32-C6, ESP32-H2, ESP32-S3 and ESP32-C3 (revision 0.3 or later)

[probe-rs]: https://probe.rs/
<!--- the search currently doesn't work on the probe-rs website --->
[chips]: https://probe.rs/targets/?q=espressif&p=0
[esp-prog]: https://docs.espressif.com/projects/esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html

## Installation

Refer to the [installation][probe-rs-installation] and [setup][probe-rs-setup] guide available on the probe-rs website.

[probe-rs-installation]: https://probe.rs/docs/getting-started/installation/
[probe-rs-setup]: https://probe.rs/docs/getting-started/probe-setup/
