# Using `esp-generate`

With all the necessary tools installed, you're ready to create your first Rust project running on an Espressif chip.

## Generating a Project

To start, launch the interactive configuration tool by running:

```shell
esp-generate --chip esp32c6 your-project-name
```

Make sure to replace the chip type and project name as needed. You can also leave them out and `esp-generate` will prompt you for them.

![Screenshot](../assets/esp-generate.png)

Adjust the options as needed for your project. See [Available Options][available-options] section of the README.

[available-options]: https://github.com/esp-rs/esp-generate?tab=readme-ov-file#available-options

> 💡 **Hint**: When using `espflash` you might want to enable `Use esp-backtrace as the panic handler.` and `Use the log crate to print messages.` under `Flashing, logging and debugging (espflash)`

> 💡 **Hint**: When you intent to use `probe-rs` instead of `espflash` you should enable `Use probe-rs to flash and monitor instead of espflash.`, then enable `Use defmt to print messages.` and `Use panic-rtt-target as the panic handler.` under `Flashing, logging and debugging (probe-rs)`

## Running the Code

Getting your code up and running is as simple as executing:

```shell
cargo run --release
```

This command compiles your application, flashes it to the target device, and starts monitoring the log output.

🎉 Congratulations — you've successfully flashed your first Rust program onto an ESP32!

You're now ready to dive deeper into developing with Rust on the ESP32 platform.
