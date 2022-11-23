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

## Missing `libtinfo.so.5`
```
thread 'main' panicked at 'Unable to find libclang: "the `libclang` shared library at /home/user/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/lib/libclang.so.15.0.0 could not be o
pened: libtinfo.so.5: cannot open shared object file: No such file or directory"', /home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```
Our current version of LLVM, 15, requires `libtinfo.so.5`. This dependency will probably be removed in our futures LLVM releases, but for the momment, please, make sure you have it installed:
- Ubuntu/Debian: `sudo apt-get install libtinfo5`
- Fedora: `sudo dnf install ncurses-compat-libs`
- openSUSE: `sudo dnf install libncurses5`
- Arch Linux: `sudo pacman -S ncurses5-compat-libs`


## Missing `ldproxy`
```
error: linker `ldproxy` not found
  |
  = note: No such file or directory (os error 2)
```

If you are triying to build a `std` application [`ldproxy`] must be installed.
```
cargo install ldproxy
```
For more information, see [ldproxy section].

[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[ldproxy section]: installation.md#ldproxy
