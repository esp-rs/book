# Troubleshooting

Here, we will present a list of common errors that may appear when building a project alongside the reason and a solution to them.

## Environment variable LIBCLANG_PATH not set

```text
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['libclang.so', 'libclang-*.so', 'libclang.so.*', 'libclang-*.so.*'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', /home/esp/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```

We need `libclang` for [`bindgen`] to generate the Rust bindings to the ESP-IDF C headers.
Make sure the environment variable `LIBCLANG_PATH` is set and pointing to our custom fork of LLVM:

- Unix:
  ```sh
  export $HOME/.espressif/tools/xtensa-esp32-elf-clang/<clang_version>-<host_triple>/esp-clang/lib
  ```
- Windows:
  ```powershell
  $Env:LIBCLANG_PATH="%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/<clang_version>-<host_triple>/esp-clang/bin/libclang.dll"
  $Env:PATH+=";%USERPROFILE%/.espressif/tools/xtensa-esp32-elf-clang/<clang_version>-<host_triple>/esp-clang/bin/"
  ```

[`bindgen`]: https://github.com/rust-lang/rust-bindgen

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

[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy


## Using a wrong Rust toolchain

```text
$ cargo build
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name ___ --print=file-names --target xtensa-esp32-espidf --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg` (exit status: 1)
  --- stderr
  error: Error loading target specification: Could not find specification for target "xtensa-esp32-espidf". Run `rustc --print target-list` for a list of built-in targets
```

If you are encountering the previous error or a similar one, you are probably not using the proper Rust toolchain, remember that for Xtensa targets, you need to use Espressif Rust fork toolchain, there are several ways to do it:
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

[toolchain override]: https://rust-lang.github.io/rustup/overrides.html#toolchain-override-shorthand
[directory override]: https://rust-lang.github.io/rustup/overrides.html#directory-overrides
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[default toolchain]: https://rust-lang.github.io/rustup/overrides.html#default-toolchain
[Overrides chapter of The rustup book]: https://rust-lang.github.io/rustup/overrides.html#overrides

## Windows

### Long path names

When using Windows, you may encounter issues building a new project if using long path names. Follow these steps to substitute the path of your project:
```powershell
subst r: <pathToYourProject>
cd r:\
```

### Missing ABI

```powershell
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

## Missing `libtinfo.so.5`

```text
thread 'main' panicked at 'Unable to find libclang: "the `libclang` shared library at /home/user/.espressif/tools/xtensa-esp32-elf-clang/esp-15.0.0-20221014-x86_64-unknown-linux-gnu/esp-clang/lib/libclang.so.15.0.0 could not be o
pened: libtinfo.so.5: cannot open shared object file: No such file or directory"', /home/user/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```
Some of the LLVM 15 releases, `esp-15.0.0-20220922` and `esp-15.0.0-20221014`, require `libtinfo.so.5`. This dependency was removed in `esp-15.0.0-20221201` LLVM releases. If you are using any of the versions that require it, make sure `libtinf5` is installed:
- Ubuntu/Debian: `sudo apt-get install libtinfo5`
- Fedora: `sudo dnf install ncurses-compat-libs`
- openSUSE: `sudo dnf install libncurses5`
- Arch Linux: `sudo pacman -S ncurses5-compat-libs`

## FAQ

### I updated my `sdkconfig.defaults` file but it doesn't appear to have had any effect

You must clean your project and rebuild for changes in the `sdkconfig.defaults` to take effect:

```shell,ignore
cargo clean
cargo build
```

### The documentation for the crates mentioned on this page is out of date or missing

Due to the [resource limits] imposed by [docs.rs], internet access is blocked while building documentation and as such we are unable to build the documentation for `esp-idf-sys` or any crate depending on it.

Instead, we are building the documentation and hosting it ourselves on GitHub Pages:

- [`esp-idf-hal` Documentation]
- [`esp-idf-svc` Documentation]
- [`esp-idf-sys` Documentation]

[resource limits]: https://docs.rs/about/builds#hitting-resource-limits
[docs.rs]: https://docs.rs
[`esp-idf-hal` documentation]: https://esp-rs.github.io/esp-idf-hal/esp_idf_hal/
[`esp-idf-svc` documentation]: https://esp-rs.github.io/esp-idf-svc/esp_idf_svc/
[`esp-idf-sys` documentation]: https://esp-rs.github.io/esp-idf-sys/esp_idf_sys/

### \*\*\*ERROR\*\*\* A stack overflow in task main has been detected.

If the second-stage bootloader reports this error, you likely need to increase the stack size for the main task. This can be accomplished by adding the following to the `sdkconfig.defaults` file:

```ignore
CONFIG_ESP_MAIN_TASK_STACK_SIZE=7000
```

In this example, we are allocating 7kB for the main task's stack.

### How can I completely disable the watchdog timer(s)?

Add to your `sdkconfig.defaults` file:

```ignore
CONFIG_INT_WDT=n
CONFIG_ESP_TASK_WDT=n
```

Recall that you must clean your project before rebuilding when modifying these configuration files.
