# RISC-V targets only

To build Rust applications for the Espressif chips based on `RISC-V` architecture, do the following:

1. Install the [`nightly`][rustup-book-channel-nightly] toolchain with the `rust-src` [component][rustup-book-components]:

    ```shell
    rustup toolchain install nightly --component rust-src
    ```
2. Set the target:
    - For `no_std` (bare-metal) applications, run:

      ```shell
      rustup target add riscv32imc-unknown-none-elf # For ESP32-C2 and ESP32-C3
      rustup target add riscv32imac-unknown-none-elf # For ESP32-C6 and ESP32-H2
      ```

      This target is currently [Tier 2][rust-lang-book--platform-support-tier2]; note the different flavors of `riscv32` target in Rust covering different [`RISC-V` extensions][wiki-riscv-standard-extensions].

    - For `std` applications:

      Since this target is currently [Tier 3][rust-lang-book--platform-support-tier3], it does not have pre-built objects distributed through `rustup` and, unlike the `no_std` target, **nothing needs to be installed**. Refer to the [*-esp-idf][rust-lang-book--platform-support--esp-idf] section of the rustc book for the correct target for your device.

      - `riscv32imc-esp-espidf` for SoCs which do not support atomics, like ESP32-C2 and ESP32-C3
      - `riscv32imac-esp-espidf` for SoCs which support atomics, like ESP32-C6, ESP32-H2, and ESP32-P4
3. To build `std` projects, you also need to install:
    - [`LLVM`][llvm-website] compiler infrastructure
    - Other [`std` development requirements][rust-esp-book-std-requirements]
    - In your project's file `.cargo/config.toml`, add the unstable Cargo [feature][cargo-book-unstable-features] `-Z build-std`. Our [template projects][rust-esp-book-write-app-generate-project] that are discussed later in this book already include this.

Now you should be able to build and run projects on the Espressif's `RISC-V` chips.

[rustup-book-channel-nightly]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[rustup-book-components]: https://rust-lang.github.io/rustup/concepts/components.html
[rust-lang-book--platform-support-tier2]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[wiki-riscv-standard-extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[rust-lang-book--platform-support-tier3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3
[rust-lang-book--platform-support--esp-idf]: https://doc.rust-lang.org/nightly/rustc/platform-support/esp-idf.html
[llvm-website]: https://llvm.org/
[cargo-book-unstable-features]: https://doc.rust-lang.org/cargo/reference/unstable.html
[rust-esp-book-write-app-generate-project]: ../writing-your-own-application/generate-project/index.md
[rust-esp-book-std-requirements]: ./std-requirements.md
