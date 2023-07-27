# Visual Studio Code

One of the more common development environments is Microsoft's [Visual Studio Code][vscode] text editor along with the [Rust Analyzer][rust-analyzer] extension.

Visual Studio Code is an open-source and cross-platform graphical text editor with a rich ecosystem of extensions. The [Rust Analyzer extension][rust-analyzer-extension] provides an implementation of the [Language Server Protocol][language-server-protocol] for Rust and additionally includes features like autocompletion, go-to definition, and more.

Visual Studio Code can be installed via most popular package managers, and installers are available on the official website. The [Rust Analyzer extension][rust-analyzer-extension] can be installed in Visual Studio Code via the built-in extension manager.

Alongside Rust Analyzer (RA), there are other extensions that might be very helpful:

- [Even Better TOML][even-better-toml] for editing TOML-based configuration files
- [crates][crates] to help manage Rust dependencies

[vscode]: https://code.visualstudio.com/
[rust-analyzer]: https://rust-analyzer.github.io/
[rust-analyzer-extension]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer
[language-server-protocol]: https://microsoft.github.io/language-server-protocol/
[even-better-toml]: https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml
[crates]: https://marketplace.visualstudio.com/items?itemName=serayuzgur.crates

## Tips and Tricks

### Using Rust Analyzer with `no_std`

If you are developing for a target that does not have `std` support, Rust Analyzer can behave strangely, often reporting various errors. This can be resolved by creating a `.vscode/settings.json` file in your project and populating it with the following:

```json
{
  "rust-analyzer.checkOnSave.allTargets": false
}
```

### Cargo hints when using custom toolchains

If you are using a custom toolchain, as you would with Xtensa targets, you can provide some hints to `cargo` via the `rust-toolchain.toml` file to improve the user experience:

```toml
[toolchain]
channel = "esp"
components = ["rustfmt", "rustc-dev"]
targets = ["xtensa-esp32-none-elf"]
```

## Other IDEs

Eventhough we have only covered VS Code because it has good support for Rust and is very popular, there are other IDEs like [CLion][clion] or [vim][vim] that also have pretty good support for Rust, but we won't be covering them here.

[cLion]: https://www.jetbrains.com/clion/
[vim]: https://www.vim.org/
