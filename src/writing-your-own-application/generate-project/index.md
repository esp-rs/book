# Generating Projects from Templates

We currently maintain two template repositories:
- [`esp-generate`][esp-generate] - `no_std` template.
- [`esp-idf-template`][esp-idf-template] - `std` template.


## `esp-generate`

`esp-generate` is project generation tool that can be used to generate an application with all the required configurations and dependencies

1. Install `esp-generate`:
    ```shell
    cargo install esp-generate
    ```
2. Generate a project based on the template, selecting the chip and the name of the project:
    ```shell
    esp-generate --chip=esp32c6 your-project
    ```
    See [Understanding `esp-generate`][understanding-esp-generate] for more details on the template project.

    When the `esp-generate` subcommand is invoked, you will be prompted with a TUI where you can select the configuration of your application. Upon completion of this process, you will have a buildable project with all the correct configurations.

3. Build/Run the generated project:
   - Use `cargo build` to compile the project using the appropriate toolchain and target.
   - Use `cargo run` to compile the project, flash it, and open a serial monitor with our target device.

[esp-generate]: https://github.com/esp-rs/esp-generate
[understanding-esp-generate]: ./esp-generate.md

## `esp-idf-template`

`esp-idf-template` is based on [`cargo-generate`][cargo-generate], a tool that allows you to create a new project based on some existing template. In our case, [`esp-idf-template`][esp-idf-template] can be used to generate an application with all the required configurations and dependencies.

1. Install `cargo generate`:
    ```shell
    cargo install cargo-generate
    ```
2. Generate a project based on the template:
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
[understanding-esp-idf-template]: ./esp-idf-template.md

## Using Dev Containers in the Templates

Both template repositories have a prompt for Dev Containers support.

Dev Containers use the [`idf-rust`][idf-rust] container image, which was explained in the [Using Container][using-container] section of the [Setting up a Development Environment][setting-env] chapter. This image provides an environment ready to develop Rust applications for Espressif chips with no installation required. Dev Containers also have integration with [Wokwi simulator][wokwi], to simulate the project, and allow flashing from the container using [`web-flash`][web-flash].

[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
[using-container]: ../../installation/using-containers.md
[wokwi]: https://wokwi.com/
[web-flash]: https://github.com/bjoernQ/esp-web-flash-server
[setting-env]: ../../installation/index.md
