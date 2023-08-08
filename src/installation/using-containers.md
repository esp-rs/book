# Using Containers

Instead of installing directly on your local system, you can host the development environment inside a container. Espressif provides the [`idf-rust`][idf-rust] image that supports both `RISC-V` and `Xtensa` target architectures and enables both `std` and `no_std` development.

You can find numerous tags for `linux/arm64`, and `linux/amd64` platforms.

For each Rust release, we generate the tag with the following naming convention:

- `<chip>_<rust-toolchain-version>`
  - For example, `esp32_1.64.0.0` contains the ecosystem for developing `std`, and `no_std` applications for `ESP32` with the `1.64.0.0` `Xtensa` Rust toolchain.

There are special cases:

- `<chip>` can be `all` which indicates compatibility with all Espressif targets
- `<rust-toolchain-version>` can be `latest` which indicates the latest release of the `Xtensa` Rust toolchain

Depending on your operating system, you can choose any container runtime, such as [Docker][docker], [Podman][podman], or [Lima][lima].

[docker]: https://www.docker.com/
[podman]: https://podman.io/
[lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
