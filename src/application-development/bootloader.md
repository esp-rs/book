# Building a Custom ESP-IDF Bootloader

The `espflash` and `cargo-espflash` include pre-built ESP-IDF [bootloaders] that are compiled using the default settings, making it easy to get started without additional configuration. However, if you require more advanced functionality or custom settings, you will need to build the bootloader yourself to tailor it to your specific needs.

To build a custom ESP-IDF bootloader:
1. [Install ESP-IDF][esp-idf-install].
2. Create a new project or go to an already existing one.
   - Using any examples under the `esp-idf/examples` directory is the easiest way.
3. Make desired changes using `idf.py menuconfig` or editing the `sdkconfig` file.
   - See [Configuration Options Reference][config-reference] for more information.
4. Build the bootloader with `idf.py set-target <CHIP_TARGET> build bootloader`.
   - The resulting booloader binary will be placed under `build/bootloader/bootloader.bin`.
5. Use the built bootloader binary in `espflash/cargo-espflash` with the `--bootloader` flag or with the [configuration file][espflash-config-file].


[bootloaders]: https://github.com/esp-rs/espflash/tree/main/espflash/resources/bootloaders
[esp-idf-install]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/index.html#manual-installation
[config-reference]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/kconfig-reference.html#configuration-options-reference
[espflash-config-file]: https://github.com/esp-rs/espflash/tree/main/espflash#configuration-file
