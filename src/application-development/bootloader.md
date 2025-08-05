# Application Startup Flow

To boot an application, Espressif devices use 2 bootloaders:
- First Stage Bootloader (ROM Bootloader): Sets up architecture-specific registers, checks the [boot mode][boot-mode] and reset reason, and loads the second stage bootloader.
- Second Stage Bootloader: Loads your application and sets up the memory(RAM, PSRAM or flash).

The second stage bootloader is required as it allows OTA support (the first stage bootloader only loads applications from a fixed offset in Flash) and also enables Flash encryption and secure boot.

For more information about the startup process, please check [ESP-IDF documentation][esp-idf-startup]. However, note that some aspects may be ESP-IDF specific.

[boot-mode]: https://docs.espressif.com/projects/esptool/en/latest/esp32c6/advanced-topics/boot-mode-selection.html?highlight=boot%20mode
[esp-idf-startup]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/startup.html

## Second Stage Bootloader

At the moment, only ESP-IDF Bootloader is supported as a second stage bootloader, this will change in the future as we plan to add support for other bootloaders.

### ESP-IDF Bootloader

Uses the [ESP image format][esp-image-format], see more information in [ESP-IDF documentation][esp-idf-second-stage-bootloader]. The ESP-IDF bootloader is used with a partition table to know where to place the binary.

[esp-idf-second-stage-bootloader]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/startup.html#second-stage-bootloader

#### Partition Tables

Flash memory of the Espressif devices can store multiple applications along with various types of data, such as calibration data, filesystems, and parameter storage. To manage this, a partition table is flashed at the default offset device.

The ESP-IDF second stage bootloader will know where to place the binary by looking at the partition table. Each entry in the partition table has a name (label), type (`app`, `data`, or something else), subtype and the offset in flash where the partition is loaded.

When working with [`espflash`][espflash], if you don't provide a second stage bootloader or partition table, `espflash` will use a default bootloader and partition table, but you can also create a [custom partition table][custom-partition-table].


[esp-image-format]: https://docs.espressif.com/projects/esptool/en/latest/esp32/advanced-topics/firmware-image-format.html
[espflash]: ../getting-started/tooling/espflash.md
[custom-partition-table]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/partition-tables.html#creating-custom-tables

#### Building a Custom ESP-IDF Bootloader

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
