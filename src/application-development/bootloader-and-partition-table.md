# Bootloader and Partition Table

## Bootloader

The `espflash` includes pre-built ESP-IDF [bootloaders] that are compiled using the default settings, making it easy to get started without additional configuration. However, if you require more advanced functionality or custom settings, you will need to build the bootloader yourself to tailor it to your specific needs. To do that, you need to:
1. [Set up esp-idf].
2. Create a new project or go to an already existing one.
3. Make desired changes using `idf.py menuconfig`.
4. Re-build with `idf.py set-target <CHIP_TARGET> build`.
5. Use the built bootloader binary with `espflash flash --bootloader <your_esp_idf_project/build/bootloader/bootloader.bin>` command.

## Partition Table

See more information in [partition-table] chapter.


[bootloaders]: https://github.com/esp-rs/espflash/tree/main/espflash/resources/bootloaders
[Set up the esp-idf]: https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/index.html#manual-installation
[partition-table]: ../introduction/startup-flow.md