# Troubleshooting

Here, we will present a list of common errors that may appear when building a project alongside the reason and a solution to them.

## Environment variable LIBCLANG_PATH not set

```sh
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['libclang.so', 'libclang-*.so', 'libclang.so.*', 'libclang-*.so.*'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', /home/esp/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```

We need `libclang` for [`bindgen`] to generate the Rust bindings to the ESP-IDF C headers.
Make sure the environment variable `LIBCLANG_PATH` is set and pointing to our custom fork of LLVM:

- Unix:
  ```sh
  export $HOME/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/lib
  ```
- Windows:
  ```powershell
  $Env:LIBCLANG_PATH="%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/bin/libclang.dll"
  $Env:PATH+=";%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/bin/"
  ```

[`bindgen`]: https://github.com/rust-lang/rust-bindgen

## Missing `libtinfo.so.5`

```sh
thread 'main' panicked at 'Unable to find libclang: "the `libclang` shared library at /home/user/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/lib/libclang.so.15.0.0 could not be o
pened: libtinfo.so.5: cannot open shared object file: No such file or directory"', /home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```

Our current version of LLVM, 15, requires `libtinfo.so.5`. This dependency will probably be removed in our future LLVM releases, but for the moment, please, make sure you have it installed:

- Ubuntu/Debian: `sudo apt-get install libtinfo5`
- Fedora: `sudo dnf install ncurses-compat-libs`
- openSUSE: `sudo dnf install libncurses5`
- Arch Linux: `sudo pacman -S ncurses5-compat-libs`

## Missing `ldproxy`

```sh
error: linker `ldproxy` not found
  |
  = note: No such file or directory (os error 2)
```

If you are trying to build a `std` application [`ldproxy`] must be installed.

```sh
cargo install ldproxy
```

For more information, see [ldproxy section].

[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[ldproxy section]: installation.md#ldproxy

## Using wrong Rust toolchain

```sh
$ cargo build
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name ___ --print=file-names --target xtensa-esp32-espidf --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg` (exit status: 1)
  --- stderr
  error: Error loading target specification: Could not find specification for target "xtensa-esp32-espidf". Run `rustc --print target-list` for a list of built-in targets
```

If you are encountering the previous error or a similar one, you are probably not using the proper Rust toolchain, remember that [for Xtensa targets, you need to use Espressif Rust fork toolchain], there are several ways to do it:

- A [toolchain override] shorthand used on the command-line: `cargo +esp`.
- Set `RUSTUP_TOOLCHAIN` environment variable to `esp`.
- Set a [directory override]: `rustup override set esp`
- Add a [rust-toolchain.toml] file to you project:
  ```toml
  [toolchain]
  channel = "esp"
  ```
- Set `esp` as [default toolchain].

For more information on toolchain overriding, see the [Overrides chapter of The rustup book].

[for Xtensa targets, you need to use Espressif Rust fork toolchain]: index.md#rust-in-xtensa-targets
[toolchain override]: https://rust-lang.github.io/rustup/overrides.html#toolchain-override-shorthand
[directory override]: https://rust-lang.github.io/rustup/overrides.html#directory-overrides
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[default toolchain]: https://rust-lang.github.io/rustup/overrides.html#default-toolchain
[Overrides chapter of The rustup book]: https://rust-lang.github.io/rustup/overrides.html#overrides

## Windows

### Long path names

When using Windows, you may encounter issues building a new project if using long path names. Follow these steps to substitute the path of your project:

```sh
subst r: <pathToYourProject>
cd r:\
```

### Missing ABI

```sh
  Compiling cc v1.0.69
error: linker `link.exe` not found
  |
  = note: The system cannot find the file specified. (os error 2)

note: the msvc targets depend on the msvc linker but `link.exe` was not found

note: please ensure that VS 2013, VS 2015, VS 2017 or VS 2019 was installed with the Visual C++ option

error: could not compile `compiler_builtins` due to previous error
warning: build failed, waiting for other jobs to finish...
error: build failed
```

The reason for this error is that we are missing the MSVC C++, hence we are not meeting the [Compile-time Requirements], please install [Visual Studio 2013 (or later) or the Visual C++ Build Tools 2019]. For Visual Studio, make sure to check the "C++ tools" and "Windows 10 SDK" options.
If using GNU ABI, install [MinGW/MSYS2 toolchain].

[Compile-time Requirements]: https://github.com/rust-lang/cc-rs#compile-time-requirements
[Visual Studio 2013 (or later) or the Visual C++ Build Tools 2019]: https://rust-lang.github.io/rustup/installation/windows.html
[MinGW/MSYS2 toolchain]: https://www.msys2.org/
