# Hardware Overview

The crates under the `esp-rs` organization include support for the [ESP32, ESP32-S, ESP32-C and ESP32-H Series SoCs][espressif-socs].

> ⚠️ **Note**:  The ESP8266 is not supported.
>
> However, the ESP8684 (ESP32-C2) and ESP8685 (ESP32-C3) are supported. Notably, the ESP8685 (ESP32-C3) is pin-compatible with the ESP8266, making it a suitable drop-in replacement.

Each SoC has its own unique features while sharing some common traits. To select the appropriate chip for your project, please use the [Espressif Product Selector][product-selector].

The Espressif portfolio is based on two different system architectures:
- [Xtensa][xtensa-architecture]: The ESP32 and ESP32-S series are based on the Xtensa architecture.
- [RISC-V][riscv-architecture]: The ESP32-C and ESP32-H series are based on the RISC-V architecture.

We won't go into the details or differences between the two architectures here. Rust's official support differs between the two architectures. Xtensa is not yet officially supported by Rust; the reason for Rust not supporting Xtensa is that Rust uses LLVM as part of its compiler infrastructure, and LLVM does not yet support Xtensa. For this reason, we maintain custom forks of both LLVM and the Rust compiler that include Xtensa support, and we are actively working to upstream our changes to enable official support in the future.

> ⚠️ **Note**: We are actively working to upstream our forks. Below is the current status:
> 1. LLVM fork: We've made significant progress recently. For details, refer to the [tracking issue][llvm-github-fork-upstream issue].
> 2. Rust compiler fork: We've submitted all feasible Xtensa patches. Further progress depends on these changes being upstreamed into LLVM.

Feel free to refer to the [Technical Documentation][espressif-docs] for more information about the different SoCs.

[espressif-socs]: https://www.espressif.com/en/products/socs
[product-selector]: https://products.espressif.com/#/
[xtensa-architecture]: https://www.cadence.com/content/dam/cadence-www/global/en_US/documents/tools/silicon-solutions/compute-ip/isa-summary.pdf
[riscv-architecture]: https://en.wikipedia.org/wiki/RISC-V
[espressif-docs]: https://www.espressif.com/en/support/documents/technical-documents
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4
