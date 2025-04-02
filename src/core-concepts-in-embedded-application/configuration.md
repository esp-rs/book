# Configuration

The [esp-config] crate provides a way to manage configuration settings.

## Usage

The full list of available options can be found in the documentation.

For example, if you want to place a function in your code in the RAM (`ESP_HAL_CONFIG_PLACE_SPI_DRIVER_IN_RAM`), which is set to `false` as default, you need to create an environment variable with the same name and modify its value:

```toml
# .cargo/config.toml

[env]
ESP_HAL_CONFIG_PLACE_SPI_DRIVER_IN_RAM="true"
```

After modifying the `.cargo/config.toml` and `[env]` section, **clean build** is recommended.

> ⚠️ **Note**: Setting the `env-vars` on the command line will have precedence over env section.

For more information see [esp-config] README.

[esp-config]: https://crates.io/crates/esp-config