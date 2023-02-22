# Generating Projects from Templates

We currently maintin two template repositories:

- [esp-template] - `no_std` template.
- [esp-idf-template] - `std` template.

Both templates are based on [cargo-generate], a tool that allows you to create a new project based on some existing template. In our case [esp-idf-template] or [esp-template] can be used to generate an application with all the required configuration and dependencies.

`cargo generate` can be installed by running:

```shell
cargo install cargo-generate
```

When the `cargo generate` subcommand is invoked, you will be prompted to answer a number of questions regarding the target of your application. Upon completion of this process you will have a buildable project with all the correct configuration.

The generated application can be built as normal using the appropriate toolchain and target simply by running `cargo build` when using either templates.

Using `cargo run` will compile the project, flash it, and open a serial monitor with our chip.

## esp-idf-template

When using the Rust standard library (`std`) you can use the [esp-idf-template] template, which will look something like:

```shell
$ cargo generate --git https://github.com/esp-rs/esp-idf-template cargo
🤷   Project Name : esp-rust-app
🔧   Destination: /home/alice/esp-rust-app ...
🔧   Generating template ...
✔ 🤷   MCU · esp32
✔ 🤷   Configure project to use Dev Containers (VS Code, GitHub Codespaces and Gitpod)? (beware: Dev Containers not available for esp-idf v4.3.2) · false
✔ 🤷   STD support · true
✔ 🤷   ESP-IDF native build version (v4.3.2 = previous stable, v4.4 = stable, mainline = UNSTABLE) · v4.4
[ 1/10]   Done: .cargo/config.toml
[ 2/10]   Done: .cargo
[ 3/10]   Done: .gitignore
[ 4/10]   Done: .vscode
[ 5/10]   Done: Cargo.toml
[ 6/10]   Done: build.rs
[ 7/10]   Done: rust-toolchain.toml
[ 8/10]   Done: sdkconfig.defaults
[ 9/10]   Done: src/main.rs
[10/10]   Done: src
🔧   Moving generated files into: `/home/alice/esp-rust-app`...
💡   Initializing a fresh Git repository
✨   Done! New project created /home/alice/esp-rust-app
```

See [Understanding esp-idf-template] for more details on the template project.

## esp-template

For bare-metal applications (`no_std`) you can instead use the [esp-template] template:

```shell
cargo generate --git https://github.com/esp-rs/esp-template
🤷   Project Name : esp-rust-app
🔧   Destination: /home/alice/esp-rust-app ...
🔧   Generating template ...
✔ 🤷   Which MCU to target? · esp32c3
✔ 🤷   Configure project to use Dev Containers (VS Code, GitHub Codespaces and Gitpod)? · false
✔ 🤷   Enable allocations via the esp-alloc crate? · false
[ 1/11]   Done: .cargo/config.toml
[ 2/11]   Done: .cargo
[ 3/11]   Done: .gitignore
[ 4/11]   Done: .vscode/settings.json
[ 5/11]   Done: .vscode
[ 6/11]   Done: Cargo.toml
[ 7/11]   Done: LICENSE-APACHE
[ 8/11]   Done: LICENSE-MIT
[ 9/11]   Done: rust-toolchain.toml
[10/11]   Done: src/main.rs
[11/11]   Done: src
🔧   Moving generated files into: `/home/alice/esp-rust-app`...
✨   Done! New project created /home/alice/esp-rust-app
```

See [Understanding esp-template] for more details on the template project.

### Using Dev Containers in the templates

Both template repositories have a prompt for Dev Containers support, when using Dev Containers in the templates it will add support for:

- [VS Code Dev Containers]
- [GitHub Codespaces]
- [Gitpod]

Dev Containers use the `idf-rust` container image that was explained in the [Using Container section of the Installing Rust chapter] and provide an environment ready to develop Rust applications for Espressif chips with no installation required. Dev Containers also have integration with [Wokwi simulator], to simulate the project, and allow flashing from the container using [web flash].

For more details about on Dev Containers, see [Dev Container section of the template Readme].

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
[VS Code Dev Containers]: https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container
[GitHub Codespaces]: https://docs.github.com/en/codespaces/developing-in-codespaces/creating-a-codespace
[Gitpod]: https://www.gitpod.io
[Using Container section of the Installing Rust chapter]: ../installation/installation.md#using-containers
[Wokwi simulator]: https://wokwi.com/
[web flash]: https://github.com/bjoernQ/esp-web-flash-server
[Dev Container section of the template Readme]: https://github.com/esp-rs/esp-template/tree/main/docs#dev-containers
[Understanding esp-template]: ./no-std-applications/understanding-esp-template.md
[Understanding esp-idf-template]: ./std-applications/understanding-esp-idf-template.md
