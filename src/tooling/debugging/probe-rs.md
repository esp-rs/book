# `probe-rs`

The [`probe-rs`][probe-rs] project is a set of tools to interact with embedded MCU's using various debug probes. It is similar to [OpenOCD][openocd], [pyOCD][pyocd], [Segger tools][segger-tools], etc. There is support for `ARM` & `RISC-V` architectures along with a collection of tools, including but not limited to:

- Debugger
  - GDB support.
  - CLI for interactive debugging.
  - VS Code extension.
- [Real Time Transfer (RTT)][rtt]
  - Similar to [app_trace component of IDF][app-trace-idf].
- Flashing algorithms

Follow the [installation][prober-rs-installation] and [setup][prober-rs-setup] instructions at the [probe-rs] website.

[probe-rs]: https://probe.rs/
[openocd]: https://openocd.org/
[pyocd]: https://pyocd.io/
[segger-tools]: https://www.segger.com/
[app-trace-idf]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/app_trace.html
[rtt]: https://wiki.segger.com/RTT
[prober-rs-installation]: https://probe.rs/docs/getting-started/installation/
[prober-rs-setup]: https://probe.rs/docs/getting-started/probe-setup/


## Permissions - Linux (TO BE REMOVED (?))
On Linux, you may run into permission issues trying to interact with Espressif probes. To avoid those: Installing the following `udev` rules and reloading should fix that issue.

```text
# Espressif dev kit FTDI
ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6010", MODE="660", GROUP="plugdev", TAG+="uaccess"

# Espressif USB JTAG/serial debug unit
ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1001", MODE="660", GROUP="plugdev", TAG+="uaccess"

# Espressif USB Bridge
ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1002", MODE="660", GROUP="plugdev", TAG+="uaccess"
```


## `USB-JTAG-SERIAL` Peripheral

Some of our recent products contain the `USB-JTAG-SERIAL` peripheral that allows for debugging without any external hardware debugger. More info on configuring the interface can be found in the official documentation for the chips that support this peripheral:
- [ESP32-C3][esp32c3-docs]
- [ESP32-C6][esp32c6-docs]
- [ESP32-H2][esp32h2-docs]
- ESP32-S3 also has the `USB-JTAG-SERIAL` peripheral but since its `Xtensa` based, it isn't yet supported by `probe-rs`.

[esp32c3-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32c6-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32h2-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32h2/api-guides/jtag-debugging/configure-builtin-jtag.html


## Flashing with `probe-rs`

`probe-rs` can be use to flash your applications to your target, there are 2 supported image formats:
- [ESP-IDF image format][idf-image]: This is the default format used by `espflash` and `cargo-espflash` and can be used in `probe-rs` using the `--format idf` argument
  - Example command for flashing an ESP32-C3: `probe-rs run --chip esp32c3 --format idf`
- `direct-boot`: Only supported in some Espressif products.
  - Example command for flashing an ESP32-C3: `probe-rs run --chip esp32c3`

The flashing command can be set as a custom Cargo runner by adding the following to your project's `.cargo/config.toml` file:

```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "probe-rs run --chip esp32c3 --format idf"
```

With this configuration, you can flash and monitor your application using `cargo run`.

[idf-image]: https://docs.espressif.com/projects/esptool/en/latest/esp32c3/advanced-topics/firmware-image-format.html

## VS Code Extension

There is a `probe-rs` extension in VS Code, see `probe-rs` [VS Code documentation][probe-rs-vscode] for details on how to install, configure and use it.

### Example `launch.json`

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "probe-rs-debug",
            "request": "launch",
            "name": "Launch",
            "cwd": "${workspaceFolder}",
            "chip": "esp32c3", //!MODIFY
            "flashingConfig": {
                "flashingEnabled": true,
                "resetAfterFlashing": true,
                "haltAfterReset": true,
                "formatOptions": {
                    "format": "idf" //!MODIFY (or remove). Valid values are: 'elf'(default), 'idf'
                }
            },
            "coreConfigs": [
                {
                    "coreIndex": 0,
                    "programBinary": "./target/riscv32imc-unknown-none-elf/debug/${workspaceFolderBasename}", //!MODIFY
                    "rttEnabled": true,
                    "rttChannelFormats": [
                        {
                            "channelNumer": "0",
                            "dataFormat": "String",
                            "showTimestamp": true,
                        }
                    ]
                }
            ]
        },
        {
            "type": "probe-rs-debug",
            "request": "attach",
            "name": "Attach",
            "cwd": "${workspaceFolder}",
            "chip": "esp32c3", //!MODIFY
            "coreConfigs": [
                {
                    "coreIndex": 0,
                    "programBinary": "./target/riscv32imc-unknown-none-elf/debug/${workspaceFolderBasename}", //!MODIFY
                    "rttEnabled": true,
                    "rttChannelFormats": [
                        {
                            "channelNumer": "0",
                            "dataFormat": "String",
                            "showTimestamp": true,
                        }
                    ]
                }
            ]
        }
    ]
}
```

> ⚠️ **Note**: The example `launch.json` uses `rtt`, which may require enabling such feature in some crates, like [`esp-println`][esp-println] and [`esp-backtrace`][esp-backtrace]

[probe-rs-vscode]: https://probe.rs/docs/tools/vscode/
[esp-println]: https://github.com/esp-rs/esp-println
[esp-backtrace]: https://github.com/esp-rs/esp-backtrace?tab=readme-ov-file#features

<!-- TODO: when probe-rs can actually debug at least a C3 with decent back traces etc, add a section here with an example config: see https://github.com/probe-rs/probe-rs/issues/877 -->
