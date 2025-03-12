# Hardware Overview

The crates under the `esp-rs` organization include support for the [ESP32, ESP32-S, ESP32-C, ESP32-H and ESP32-P Series SoCs][espressif-socs].

> ⚠️ **Note**:  The ESP8266 is not supported. However, you can replace it with an ESP32-C3, which is pin-compatible with the ESP8266.

Each SoC has its own unique features while sharing some common traits. To select the appropriate chip for your project, please use the [ESP Product Selector][product-selector].

The Espressif portfolio is based on two different system architectures:
- [Xtensa][xtensa-architecture]: The ESP32 and ESP32-S series are Xtensa-based, with at least their main core using Xtensa.
- [RISC-V][riscv-architecture]: The ESP32-C, ESP32-H, and ESP32-P series are RISC-V-based.

We won't go into the details or differences between the two architectures here. Rust's official support differs between the two architectures. Xtensa is not yet officially supported, though we are actively working to add support.

Feel free to refer to the [Technical Documentation][espressif-docs] for more information about the different SoCs.

[espressif-socs]: https://www.espressif.com/en/products/socs
[product-selector]: https://products.espressif.com/#/
[xtensa-architecture]: https://www.cadence.com/content/dam/cadence-www/global/en_US/documents/tools/silicon-solutions/compute-ip/isa-summary.pdf
[riscv-architecture]: https://en.wikipedia.org/wiki/RISC-V
[espressif-docs]: https://www.espressif.com/en/support/documents/technical-documents
