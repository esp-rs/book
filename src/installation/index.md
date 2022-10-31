# Installation

With an understanding of the ecosystem surrounding Rust on Espressif chips, we're able to move on to actual development. If you are not aware of the two possible development approaches or do not understand the differences between writing `std` and `no_std` applications, please first read the [Ecosystem Overview] chapter.

The dependencies required to build Rust applications for ESP Chips depends on the approach that we take:
- For `no_std` applications:
  - GCC Toolchain: To serve as a C compiler.
  - Rust Toolchain: Used to compile our Rust code.
    - `LLVM`: The Rust compiler uses `LLVM` as codegen backend.
- For `std` applications:
  - GCC Toolchain: To serve as a C compiler.
  - Rust Toolchain: Used to compile our Rust code.
    - `LLVM`: The Rust compiler uses `LLVM` as codegen backend.
  - [ESP-IDF]: Espressif IoT Development Framework.
  - [ldproxy] crate:  Simple tool to forward linker arguments given to [ldproxy] to the actual linker executable. The crate can be found in the [esp-rs/embuild] repository.

In this chapter, we will cover how to properly install the correct Rust compiler and toolchain for our ESP chips.

[Ecosystem Overview]: ../overview/index.md
[ESP-IDF]: https://github.com/espressif/esp-idf
[`std` overview]: src\overview\using-the-standard-library.md
[ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-rs/embuild]: https://github.com/esp-rs/embuild
