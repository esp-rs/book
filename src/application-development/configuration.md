# Configuration

The [`esp-config`][esp-config] crate provides a way to manage additional configuration settings that don't fit into Cargo features, for `esp-*` crates.

## Finding Available Configuration

The full list of available options can be found in the [documentation] of the crate you are interested. For example, these are [`esp-hal`'s configuration options][esp-hal-config] for the ESP32-C6.

## Usage

While creating your project, you may need to configure some additional advanced parameters, for example, place a peripheral in RAM for better performance or change the size of RX/TX queue in some crate. In order to do so you will need to configure some settings provided by the `esp-config`.

You are free to do this in two ways:

- By setting the environment variable. For example, in `esp-hal` if you want to place anonymous symbols in RAM (`ESP_HAL_CONFIG_PLACE_ANON_IN_RAM`), which is set to `false` by default, you need to create an environment variable with the same name and modify its value.

- By setting the required parameter in `.cargo/config.toml` (this method also sets the environment variable):
    ```toml
    # .cargo/config.toml

    [env]
    ESP_HAL_CONFIG_PLACE_ANON_IN_RAM="true"
    ```
    After modifying the `.cargo/config.toml` and `[env]` section, **clean build** is recommended.

> ⚠️ **Note**: Setting environment variables on the command line will take precedence over the `[env]` section.

## Multiple Configurations

Depending on your application, you may find yourself wanting to support different boards/chips/targets in the same project. In this case, you may have specific options to set for each target. To do this, we recommend the following setup.

* A baseline `.cargo/config.toml` which has your typical build modifying flags (Cargo will _always_ read and respect this file, regardless of other `--config`'s passed)
* A config file for each configuration in `.cargo/`
* (**Recommended**) A Cargo [alias] to build with the given config, for example `run-config-a = "run --config=./.cargo/config_a.toml --release"`, but for simple cases you can pass `--config` on the CLI.

Check out [this example repo] for a more comprehensive look at multi-config projects.

## Defining Your Own Config Options

You may also want to define certain configuration options in your project. To do so, it is necessary to declaratively define these options, their default values, and other parameters and checks inside an `esp_config.yml` file. Details can be found in the [Defining Configuration Options] section of the project repository.

[documentation]: https://docs.espressif.com/projects/rust/
[esp-config]: https://crates.io/crates/esp-config
[Defining Configuration Options]: https://github.com/esp-rs/esp-hal/tree/main/esp-config#defining-configuration-options
<!-- TODO, can we get latest to work here instead of 1.0.0-rc.0? -->
[esp-hal-config]: https://docs.espressif.com/projects/rust/esp-hal/1.0.0-rc.0/esp32c6/esp_hal/index.html#additional-configuration
[this example repo]: https://github.com/bjoernQ/esp-hal-multiconfig-example/tree/main
[alias]: https://doc.rust-lang.org/cargo/reference/config.html#alias
