# Generating Projects from Templates

We currently maintain two template repositories:
- [`esp-template`][esp-template] - `no_std` template.
- [`esp-idf-template`][esp-idf-template] - `std` template.

Both templates are based on [`cargo-generate`][cargo-generate], a tool that allows you to create a new project based on some existing template. In our case, [`esp-idf-template`][esp-idf-template] or [`esp-template`][esp-template] can be used to generate an application with all the required configurations and dependencies.

1. Install `cargo generate`:
    ```shell
    cargo install cargo-generate
    ```
2. Generate a project based on one of the templates:
    - `esp-template`:
        ```shell
        cargo generate esp-rs/esp-template
        ```
        See [Understanding `esp-template`][understanding-esp-template] for more details on the template project.
    - `esp-idf-template`:
        ```shell
        cargo generate esp-rs/esp-idf-template cargo
        ```
        See [Understanding `esp-idf-template`][understanding-esp-idf-template] for more details on the template project.

    When the `cargo generate` subcommand is invoked, you will be prompted to answer several questions regarding the target of your application. Upon completion of this process, you will have a buildable project with all the correct configurations.

3. Build/Run the generated project:
   - Use `cargo build` to compile the project using the appropriate toolchain and target.
   - Use `cargo run` to compile the project, flash it, and open a serial monitor with our target device.

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
[understanding-esp-template]: ./esp-template.md
[understanding-esp-idf-template]: ./esp-idf-template.md

## Using Dev Containers in the Templates

Both template repositories have a prompt for Dev Containers support, see details in [Dev Containers][dev-container] section of the template README.

Dev Containers use the [`idf-rust`][idf-rust] container image, which was explained in the [Using Container][using-container] section of the [Setting up a Development Environment][setting-env] chapter. This image provides an environment ready to develop Rust applications for Espressif chips with no installation required. Dev Containers also have integration with [Wokwi simulator][wokwi], to simulate the project, and allow flashing from the container using [`web-flash`][web-flash].

[dev-container]: https://github.com/esp-rs/esp-template/tree/main/docs#dev-containers
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
[using-container]: ../../installation/using-containers.md
[wokwi]: https://wokwi.com/
[web-flash]: https://github.com/bjoernQ/esp-web-flash-server
[setting-env]: ../../installation/index.md
