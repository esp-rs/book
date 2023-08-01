# Setting Up a Development Environment

At the moment, Espressif SoCs are based on two different architectures: `RISC-V` and `Xtensa`. Both architectures support `std` and `no_std` approaches.

To set up the development environment, do the following:

1. [Install Rust][install-rust]
2. Install requirements based on your target(s)
    - [`RISC-V` targets only][risc-v-targets]
    - [`RISC-V` and `Xtensa` targets][rics-v-xtensa-targets]

Regardless of the target architecture, for `std` development also don't forget to install [`std` Development Requirements][rust-esp-book-std-requirements].

Please note that you can host the development environment in a [container][use-containers].

[install-rust]: ./rust.md
[risc-v-targets]: ./riscv.md
[rics-v-xtensa-targets]: ./riscv-and-xtensa.md
[rust-esp-book-std-requirements]: ./std-requirements.md
[use-containers]: ./using-containers.md
