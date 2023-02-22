# Text Editors and IDEs

While an often contentious subject, using the right development environment can make a significant impact on your productivity with a given programming language. Below can be found a curated list of what we feel are the best options.

## Visual Studio Code

One of the more common development environments is Microsoft's [Visual Studio Code] text editor along with the [Rust Analyzer] extension.

Visual Studio Code is an open-source and cross-platform graphical text editor with a rich ecosystem of extensions. The [Rust Analyzer extension] provides an implementation of the [Language Server Protocol] for Rust and additionally includes features like autocompletion, go-to definition, and more.

Visual Studio Code can be installed via most popular package managers, and installers are available on the official website. The [Rust Analyzer extension] can be installed in Visual Studio Code via the built-in extension manager.

Alongside Rust Analyzer (RA), there are other extensions that might be very helpful:

- [Even Better TOML] for editing TOML-based configuration files
- [crates] to help manage Rust dependencies

[visual studio code]: https://code.visualstudio.com/
[rust analyzer]: https://rust-analyzer.github.io/
[Rust Analyzer extension]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer
[language server protocol]: https://microsoft.github.io/language-server-protocol/
[Even Better TOML]: https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml
[crates]: https://marketplace.visualstudio.com/items?itemName=serayuzgur.crates

### Tips and Tricks

If you are developing for a target that does not have `std` support Rust Analyzer can behave strangely, often reporting various errors. This can be resolved by creating a `.vscode/settings.json` file in your project and populating it with the following:

```json
{
  "rust-analyzer.checkOnSave.allTargets": false
}
```

If you are using a custom toolchain, as you would with Xtensa targets, you can provide some hints to `cargo` via the `rust-toolchain.toml` file to improve the user experience:

```toml
[toolchain]
channel = "esp"
components = ["rustfmt", "rustc-dev"]
targets = ["xtensa-esp32-none-elf"]
```

## CLion

[CLion] is a cross-platform IDE for C and C++ from [JetBrains].

[CLion]: https://www.jetbrains.com/clion/
[JetBrains]: https://www.jetbrains.com/

## IntelliJ


## vim

[vim] is a highly configurable text editor based on vi that also [supports Rust Analyzer].

[vim]: https://www.vim.org/
[supports Rust Analyzer]: https://rust-analyzer.github.io/manual.html#vimneovim
