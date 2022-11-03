# Understanding esp-idf-template

Now that we know how to [generate a std project], let's inspect what the generated project contains and try to understand every part of it.

## Inspecting the generated Project

When creating a project from [esp-idf-template] using:
- MCU: `esp32c3`
- ESP-IDF version: `v4.4`
- STD support: `true`
- Devcontainer support: `false`

It should generate a file structure like this:

```text
├── build.rs
├── .cargo
│   └── config.toml
├── Cargo.toml
├── .gitignore
├── rust-toolchain.toml
├── sdkconfig.defaults
└── src
    └── main.rs
```

Before going further let's see what these files are for.

- [.gitignore]
    - tells `git` which folders and files to ignore
- [Cargo.toml]
    - the usual Cargo manifest declaring some meta-data and dependencies of the project
- LICENSE-APACHE, LICENSE_MIT
    - those are the most common licenses used in the Rust ecosystem
    - if you want to apply a different license you can delete these files and change the license in `Cargo.toml`
- [rust-toolchain.toml]
    - defines which Rust toolchain to use
    - depending on your target this will use `nightly` or `esp`
- [.cargo/config.toml]
    - the Cargo configuration
    - contains our target
    - contains `runner = "espflash --monitor"` - this means you can just use `cargo run` to flash and monitor your code
    - contains the linker to use, in our case, [`ldproxy`]
    - contains the unstable `build-std` cargo feature enabled.
    - contains the `ESP-IDF-VERSION` envrionment variable that tells [`esp-idf-sys`] which ESP-IDF version the project will use.
- src/main.rs
    - the main source file of the newly created project
    - we will examine its content in the next section
- [build.rs]
    - propagates linker arguments for `ldproxy`.
- [sdkconfig.defaults]
    - contains the overriden values from the ESP-IDF defaults.

## `main.rs`

```rust,ignore
use esp_idf_sys as _; // If using the `binstart` feature of `esp-idf-sys`, always keep this module imported

fn main() {
    esp_idf_sys::link_patches();
    println!("Hello, world!");
}

```
The first line its an import that defines the esp-idf entry-point when the root crate is a binary crate that defines a main function.

Then, we have an usual main function with two lines on it:
- A call to `esp_idf_sys::link_patches` function that makes sure that a few patches to the ESP-IDF which are implemented in Rust are linked to the final executable.
- We print in our console the famous "Hello World!".

[.gitignore]: https://git-scm.com/docs/gitignore
[Cargo.toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain.toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[.cargo/config.toml]: https://doc.rust-lang.org/cargo/reference/config.html
[generate a std project]: ../generate-project-from-template.md#esp-idf-template
[esp-idf-template]: https://github.com/esp-rs/esp-template
[`esp-idf-sys`]: https://github.com/esp-rs/esp-idf-sys
[`ldproxy`]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[build.rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[sdkconfig.defaults]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/build-system.html#custom-sdkconfig-defaults
