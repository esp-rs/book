# Troubleshooting

This chapter lists certain questions and common problems we have encountered over time, along with their solutions. This page collects common issues independent of the chosen ESP ecosystem. If you can't find your issue listed here, feel free to open an issue in the appropriate repository or ask on our [Matrix room][matrix].

[matrix]: https://matrix.to/#/#esp-rs:matrix.org

## Using the Wrong Rust Toolchain

```text
$ cargo build
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name ___ --print=file-names --target xtensa-esp32-espidf --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg` (exit status: 1)
  --- stderr
  error: Error loading target specification: Could not find specification for target "xtensa-esp32-espidf". Run `rustc --print target-list` for a list of built-in targets
```

If you are encountering the previous error or a similar one, you are probably not using the proper Rust toolchain. Remember that for `Xtensa` targets, you need to use Espressif Rust fork toolchain, there are several ways to do it:
- A [toolchain override][toolchain-override] shorthand used on the command-line: `cargo +esp`.
- Set `RUSTUP_TOOLCHAIN` environment variable to `esp`.
- Set a [directory override][directory-override]: `rustup override set esp`
- Add a [`rust-toolchain.toml`][rust-toolchain-toml] file to you project:
  ```toml
  [toolchain]
  channel = "esp"
  ```
- Set `esp` as [default toolchain][default-toolchain].

For more information on toolchain overriding, see the [Overrides chapter][overrides-rust-book] of The rustup book.

[toolchain-override]: https://rust-lang.github.io/rustup/overrides.html#toolchain-override-shorthand
[directory-override]: https://rust-lang.github.io/rustup/overrides.html#directory-overrides
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[default-toolchain]: https://rust-lang.github.io/rustup/overrides.html#default-toolchain
[overrides-rust-book]: https://rust-lang.github.io/rustup/overrides.html#overrides

## Windows

### Long Path Names

When using Windows, you may encounter issues building a new project if using long path names.
Moreover - and if you are trying to build a `std` application - the build will fail with a hard error if your project path
is longer than ~ 10 characters.

To workaround the problem, you need to shorten your project name, and move it to the drive root, as in e.g. `C:\myproj`.
Note also that while using the Windows `subst` utility (as in e.g. `subst r: <pathToYourProject>`) might look like an easy
solution for using short paths during build while still keeping your project location intact,
it simply *does not work*, as the short, substituted paths are expanded to their actual (long) locations by the Windows APIs.

Another alternative is to install Windows Subsystem for Linux (WSL), move your project(s) inside the native Linux file partition,
build inside WSL and only flash the compiled MCU ELF file from outside of WSL.

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

The reason for this error is that we are missing the MSVC C++, hence we aren't meeting the [Compile-time Requirements]. Please,  install [Visual Studio 2013 (or later) or the Visual C++ Build Tools 2019]. For Visual Studio, make sure to check the "C++ tools" and "Windows 10 SDK" options.
If using GNU ABI, install [MinGW/MSYS2 toolchain].

[Compile-time Requirements]: https://github.com/rust-lang/cc-rs#compile-time-requirements
[Visual Studio 2013 (or later) or the Visual C++ Build Tools 2019]: https://rust-lang.github.io/rustup/installation/windows.html
[MinGW/MSYS2 toolchain]: https://www.msys2.org/
