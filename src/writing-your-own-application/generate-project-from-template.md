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
$ cargo generate esp-rs/esp-idf-template cargo
🤷   Project Name: esp-rust-app
🔧   Destination: /home/sergio/esp-rust-app ...
🔧   project-name: esp-rust-app ...
🔧   Generating template ...
✔ 🤷   Which MCU to target? · esp32
✔ 🤷   Use the default template values? · true
defaults: true
🔧   Moving generated files into: `/home/sergio/esp-rust-app`...
Initializing a fresh Git repository
✨   Done! New project created /home/sergio/esp-rust-app
```
See [Understanding esp-idf-template] for more details on the template project.
## esp-template

For bare-metal applications (`no_std`) you can instead use the [esp-template] template:

```shell
$ cargo generate -a esp-rs/esp-template
🤷   Project Name: esp-rust-app
🔧   Destination: /home/sergio/esp-rust-app ...
🔧   project-name: esp-rust-app ...
🔧   Generating template ...
✔ 🤷   Which MCU to target? · esp32c3
✔ 🤷   Enable allocations via the esp-alloc crate? · false
✔ 🤷   Configure project to support Wokwi simulation with Wokwi VS Code extension? · false
✔ 🤷   Configure project to use Dev Containers (VS Code and GitHub Codespaces)? · false

💡 When using `espflash` version 1.x you need to edit `.cargo/config.toml`
💡 Make the runner look like this `runner = "espflash --monitor"`

Use `cargo run` to flash and run your code

🔧   Moving generated files into: `/home/sergio/esp-rust-app`...
Initializing a fresh Git repository
✨   Done! New project created /home/sergio/esp-rust-app
```
See [Understanding esp-template] for more details on the template project.

### Using Dev Containers in the templates

Both template repositories have a prompt for Dev Containers support, when using Dev Containers in the templates it will add support for:
-  [VS Code Dev Containers]
-  [GitHub Codespaces]

Dev Containers use the `idf-rust` container image that was explained in the [Container section of the Installing Rust chapter] and provide an environment ready to develop Rust applications for Espressif chips with no installation required. Dev Containers also have integration with [Wokwi simulator], to simulate the project, and allow flashing from the container using [web flash].

For more details about on Dev Containers, see [Dev Container section of the template Readme].

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
[VS Code Dev Containers]: https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container
[GitHub Codespaces]: https://docs.github.com/en/codespaces/developing-in-codespaces/creating-a-codespace
[Container section of the Installing Rust chapter]: ../installation/index.md#using-containers
[Wokwi simulator]: https://wokwi.com/
[web flash]: https://github.com/bjoernQ/esp-web-flash-server
[Dev Container section of the template Readme]: https://github.com/esp-rs/esp-template/tree/main/docs#dev-containers
[Understanding esp-template]: ./no-std-applications/understanding-esp-template.md
[Understanding esp-idf-template]: ./std-applications/understanding-esp-idf-template.md
