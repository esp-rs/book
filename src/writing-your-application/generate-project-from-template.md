# Creating Applications

With the appropriate Rust compiler and toolchain installed, you're now ready to create an application. As outlined below, there are essentially two ways to do this: generating from a template or starting from scratch using only `cargo`.

## Using `cargo-generate`

The [cargo-generate] subcommand allows you to create a new project based on some existing template. In our case [esp-idf-template] or [esp-template] can be used to generate an application with all the required configuration and dependencies.

`cargo generate` can be installed by running:

```shell
$ cargo install cargo-generate
```

When the `cargo generate` subcommand is invoked, you will be prompted to answer a number of questions regarding the target of your application. Upon completion of this process you will have a buildable project with all the correct configuration.

The generated application can be built as normal using the appropriate toolchain and target simply by running `cargo build` when using either template.

### esp-idf-template

When using the Rust standard library (`std`) you can use the [esp-idf-template] template, which will look something like:

```shell
$ cargo generate --git https://github.com/esp-rs/esp-idf-template cargo
🤷   Project Name : esp-rust-app
🔧   Generating template ...
✔ 🤷   Rust toolchain (beware: nightly works only for esp32c3!) · esp
✔ 🤷   STD support · true
✔ 🤷   ESP-IDF native build version (stable = 4.3.1, upcoming = 4.4, master = 5.0; applicable only with `cargo build --features native`) · stable
✔ 🤷   MCU · esp32
[1/9]   Done: .cargo/config.toml
[2/9]   Done: .cargo
[3/9]   Done: .gitignore
[4/9]   Done: Cargo.toml
[5/9]   Done: build.rs
[6/9]   Done: rust-toolchain.toml
[7/9]   Done: sdkconfig.defaults
[8/9]   Done: src/main.rs
[9/9]   Done: src
🔧   Moving generated files into: `/Users/alice/esp-rust-app`...
✨   Done! New project created /Users/alice/esp-rust-app
```

### esp-template

For bare-metal applications (`no_std`) you can instead use the [esp-template] template:

```shell
$ cargo generate --git https://github.com/esp-rs/esp-template
🤷   Project Name : esp-rust-app
🔧   Generating template ...
✔ 🤷   Which MCU to target? · esp32c3
✔ 🤷   Configure project to use Dev Containers (VS Code, GitHub Codespaces and Gitpod)? · true
[ 1/25]   Done: .cargo/config.toml
[ 2/25]   Done: .cargo
[ 3/25]   Done: .devcontainer/Dockerfile
[ 4/25]   Done: .devcontainer/devcontainer.json
[ 5/25]   Done: .devcontainer
[ 6/25]   Done: .dockerignore
[ 7/25]   Done: .gitdpod.Dockerfile
[ 8/25]   Done: .gitdpod.yml
[ 9/25]   Done: .gitignore
[10/25]   Done: .vscode/launch.json
[11/25]   Done: .vscode/settings.json
[12/25]   Done: .vscode/tasks.json
[13/25]   Done: .vscode
[14/25]   Done: Cargo.toml
[15/25]   Done: LICENSE-APACHE
[16/25]   Done: LICENSE-MIT
[17/25]   Done: docs/README.md
[18/25]   Done: docs
[19/25]   Done: rust-toolchain.toml
[20/25]   Done: scripts/build.sh
[21/25]   Done: scripts/flash.sh
[22/25]   Done: scripts/run-wokwi.sh
[23/25]   Done: scripts
[24/25]   Done: src/main.rs
[25/25]   Done: src
🔧   Moving generated files into: `/Users/alice/esp-rust-app`...
✨   Done! New project created /Users/alice/esp-rust-app
```

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
