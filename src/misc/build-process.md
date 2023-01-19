
## Rust in RISC-V targets

Hence, the dependencies required to develop Rust applications in `RISC-V` targets are:
- Rust Toolchain with the proper target: Used to compile our Rust code.
- `LLVM`: Used as codegen backend by the Rust compiler.
- \[_Optional_\] `GCC` Toolchain: `GCC` linker can be used.
  - `GCC` is marked as optional as you can also use `LLVM` linker.

## Rust in Xtensa targets

Hence, the dependencies required to develop Rust applications in `Xtensa` targets are:
- Rust Toolchain: Used to compile our Rust code.
  - _We need to use our custom fork with `Xtensa` support_.
- `LLVM`: Used as codegen backend by the Rust compiler.
  - _We need to use our custom fork with `Xtensa` support_.
- `GCC` Toolchain: `GCC` linker is used in our Rust applications as it supports all our targets architectures (`Xtensa` and `RISC-V`).
