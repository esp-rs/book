# Text Editors and IDEs

While an often contentious subject, using the right development environment can make a significant impact on your productivity with a given programming language. Below can be found a curated list of what we feel are the best options.

## Visual Studio Code

One of the more common development environents is Microsoft's [Visual Studio Code] text editor along with the [Rust Analyzer] extension.

Visual Studio Code is an open-source and cross-platform graphical text editor with a rich ecosystem of extensions. The [Rust Analyzer extension] provides an implementation of the [Language Server Protocol] for Rust, and additionally includes features like autocompletion, go to definition, and more.

Visual Studio Code can be installed via most popular package managers, and installers are available on the official website. The [Rust Analyzer extension] can be installed in Visual Studo Code via the built-in extension manager.

[visual studio code]: https://code.visualstudio.com/
[rust analyzer]: https://rust-analyzer.github.io/
[Rust Analyzer extension]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer
[language server protocol]: https://microsoft.github.io/language-server-protocol/

### Tips and Tricks

If you are developing for a target which does not have `std` support Rust Analyzer can behave strangely, often reporting various errors. This can be resolved by creating a `.vscode/settings.json` file in your project and populating it with the following:

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

[clion]: https://www.jetbrains.com/clion/
[clion rust plugin]: https://www.jetbrains.com/help/clion/rust-support.html
[jetbrains]: https://www.jetbrains.com/

## IntelliJ

[intellij]: https://www.jetbrains.com/idea/
[intellij rust plugin]: https://intellij-rust.github.io/

## vim

[vim]: https://www.vim.org/
[rust.vim]: https://github.com/rust-lang/rust.vim/
