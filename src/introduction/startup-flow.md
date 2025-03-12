# Application Startup Flow

In order to boot an application, Espressif devices use 2 bootloaders:
- First Stage Bootloader (ROM Bootloader): Setups architecture-specific registers, checks the [boot mode][boot-mode] and reset reason, and loads the second stage bootloader.
- Second Stage Bootloader: Loads the [partition table][partition-table] and your application.

The second stage bootloader is required as it allows OTA support (first stage bootloader only load applications from a fixed offset in Flash) and also for Flash encrytion and secure boot.

For more information about the startup process, please check [ESP-IDF documentation][esp-idf-startup]. However, note that some aspects may be ESP-IDF specific.

[boot-mode]: https://docs.espressif.com/projects/esptool/en/latest/esp32c6/advanced-topics/boot-mode-selection.html?highlight=boot%20mode
[partition-table]: #partition-tables
[esp-idf-startup]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/startup.html

### Second Stage Bootloader Alternatives

#### ESP-IDF Bootloader

Uses the [ESP image format][esp-image-format], see more information in [ESP-IDF documentation][esp-idf-second-stage-bootloader]

[esp-idf-second-stage-bootloader]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/startup.html#second-stage-bootloader

#### MCUboot

See [MCUboot documentation][mcuboot-docs]

[mcuboot-docs]: https://docs.mcuboot.com/

## Partition Tables

Flash memory of the Espressif devices can store multiple applications along with various types of data, such as calibration data, filesystems, and parameter storage. To manage this, a partition table is flashed at the default offset device.

The second stage bootloader will know where to place the binary by looking at the partition table. Each entry in the partition table has a name (label), type (app, data, or something else), subtype and the offset in flash where the partition is loaded.

When working with [`espflash`][espflash], if you dont provide a second stage bootloader or partition table, `espflash` will use a default bootloader and partition table, but you can also create a [custom partition table][custom-partition-table]

[esp-image-format]: https://docs.espressif.com/projects/esptool/en/latest/esp32/advanced-topics/firmware-image-format.html
[espflash]: ../getting-started/tooling/espflash.md
[custom-partition-table]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/partition-tables.html#creating-custom-tables
