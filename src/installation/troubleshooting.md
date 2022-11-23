# Troubleshooting

Here, we will cover some of the common errors that may appear, the reason and a solution to them.

## Environment variable LIBCLANG_PATH not set
```
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['libclang.so', 'libclang-*.so', 'libclang.so.*', 'libclang-*.so.*'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', /home/esp/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```
Make sure the environment variable `LIBCLANG_PATH` is set and pointing to our custom fork of LLVM:
- Unix:
  ```
  export $HOME/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/lib
  ```
- Windows:
  ```
  $Env:LIBCLANG_PATH="%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/bin/libclang.dll"
  $Env:PATH+=";%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/bin/"
  ```
