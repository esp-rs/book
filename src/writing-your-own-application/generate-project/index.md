# Generating Projects from Templates

We currently maintain two template repositories:
- [esp-template] - `no_std` template.
- [esp-idf-template] - `std` template.

Both templates are based on [cargo-generate], a tool that allows you to create a new project based on some existing template. In our case [esp-idf-template] or [esp-template] can be used to generate an application with all the required configuration and dependencies.

1. Install `cargo generate`:
    ```shell
    cargo install cargo-generate
    ```
2. Generate a project based in one of the templates:
    - esp-template:
        ```shell
        cargo generate -a esp-rs/esp-template
        ```
        See [Understanding esp-template] for more details on the template project.
    - esp-idf-template:
        ```shell
        cargo generate esp-rs/esp-idf-template cargo
        ```
        See [Understanding esp-idf-template] for more details on the template project.

    When the `cargo generate` subcommand is invoked, you will be prompted to answer a number of questions regarding the target of your application. Upon completion of this process, you will have a buildable project with all the correct configuration.

3. Build/Run the generated project:
   - Using `cargo build` will compile the project using the appropriate toolchain and target.
   - Using `cargo run` will compile the project, flash it, and open a serial monitor with our chip.

## Using Dev Containers in the templates

Both template repositories have a prompt for Dev Containers support, when using Dev Containers in the templates it will add support for:
-  [VS Code Dev Containers]
-  [GitHub Codespaces]

Dev Containers use the [`idf-rust` container image], that was explained in the [Container section] of the installation chapter, and provide an environment ready to develop Rust applications for Espressif chips with no installation required. Dev Containers also have integration with [Wokwi simulator], to simulate the project, and allow flashing from the container using [web flash].

For more details about on Dev Containers, see [Dev Container] section of the template README.

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
[VS Code Dev Containers]: https://code.visualstudio.com/docs/remote/containers#_quick-start-open-an-existing-folder-in-a-container
[GitHub Codespaces]: https://docs.github.com/en/codespaces/developing-in-codespaces/creating-a-codespace
[Container section]: ../../installation/index.md#using-containers
[Wokwi simulator]: https://wokwi.com/
[web flash]: https://github.com/bjoernQ/esp-web-flash-server
[Dev Container]: https://github.com/esp-rs/esp-template/tree/main/docs#dev-containers
[Understanding esp-template]: ./esp-template.md
[Understanding esp-idf-template]: ./esp-idf-template.md
[`idf-rust` container image]: https://hub.docker.com/r/espressif/idf-rust/tags
