# `probe-rs`

The [`probe-rs`][probe-rs] project is a set of tools to interact with embedded MCUs using various debug probes. It is similar to [OpenOCD][openocd], [pyOCD][pyocd], [Segger tools][segger-tools], etc. There is support for Xtensa & RISC-V architectures along with a collection of tools, including but not limited to:

- Flashing algorithms
- Debugger
  - GDB support.
  - CLI for interactive debugging.
  - VS Code extension.
- [Real Time Transfer (RTT)][rtt]
  - Similar to [`app_trace` component of IDF][app-trace-idf].

Espressif products containing the `USB-JTAG-SERIAL` peripheral can use `probe-rs` without any external hardware.

> ⚠️ **Note**: `USB-JTAG-SERIAL` peripheral is available in ESP32-C6, ESP32-H2, ESP32-S3 and ESP32-C3(revision 0.3 or later)

[probe-rs]: https://probe.rs/
[openocd]: https://openocd.org/
[pyocd]: https://pyocd.io/
[segger-tools]: https://www.segger.com/
[app-trace-idf]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/app_trace.html
[rtt]: https://wiki.segger.com/RTT

## Insallation

Follow the [installation][prober-rs-installation] and [setup][prober-rs-setup] instructions at the [`probe-rs`][probe-rs] website.

[prober-rs-installation]: https://probe.rs/docs/getting-started/installation/
[prober-rs-setup]: https://probe.rs/docs/getting-started/probe-setup/

## Flashing an Application

Assuming you have built an ELF binary by other means already, `probe-rs` can be used to download it to your device with:


```shell
probe-rs run
```

## Using `probe-rs` as a Cargo Runner

The flashing command can be set as a custom Cargo runner by adding the following to your project's `.cargo/config.toml` file:

```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "probe-rs run"
```

With this configuration, you can flash and monitor your application using `cargo run`.

This is automatically done when the project is generated via [`esp-generate`][esp-generate].

[esp-generate]: esp-generate.md

## `cargo-flash` and `cargo-embed`

`probe-rs` comes along with these two tools:
- [`cargo-flash`][cargo-flash]: A flash tool that downloads your binary to the target and runs it.
- [`cargo-embed`][cargo-embed]: Superset of `cargo-flash` that also allows opening an RTT terminal or a GDB server. A [configuration file][cargo-embed-config] can used to define the behavior.

[cargo-flash]: https://probe.rs/docs/tools/cargo-flash/
[cargo-embed]: https://probe.rs/docs/tools/cargo-embed/
[cargo-embed-config]: https://probe.rs/docs/tools/cargo-embed/#configuration
