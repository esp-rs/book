# Configuration

The [`esp-config`][esp-config] crate provides a way to manage additional configuration settings.

## Usage
While creating their project, the user may need to configure some additional advanced parameters, for example, place a peripheral in RAM for better performance or change the size of RX/TX queue when working with Wi-Fi and so on. In order to do so they will need to configure some settings provided by the `esp-config`. 

The user is free to do this in two ways: 
- By setting the environment variable. For example, in `esp-hal` if you want to place anonymous symbols in the RAM (`ESP_HAL_CONFIG_PLACE_ANON_IN_RAM`), which is set to `false` as default, you need to create an environment variable with the same name and modify its value.

- By setting the required parameter in `.cargo/config.toml` (this method also sets the environment variable):
    ```toml
    # .cargo/config.toml

    [env]
    ESP_HAL_CONFIG_PLACE_ANON_IN_RAM="true"
    ```
    After modifying the `.cargo/config.toml` and `[env]` section, **clean build** is recommended.

The full list of available options can be found in the [documentation] of the crate you are interested.

> ⚠️ **Note**: Setting the environment variables on the command line will have precedence over env section.

## Defining Config Options
The user may also want to define certain configuration options in their project. To do so, in the pre-created `esp_config.yml` file, it is necessary to declaratively define these options, their default values, and other parameters. Details can be found in the [Defining Configuration Options] section of the project repository.

[documentation]: https://docs.espressif.com/projects/rust/
[esp-config]: https://crates.io/crates/esp-config
[Defining Configuration Options]: https://github.com/esp-rs/esp-hal/tree/main/esp-config#defining-configuration-options