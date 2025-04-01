# Bootloader and Partition Table

## Bootloader

The `espflash` includes pre-built `esp-idf` [bootloaders] that are compiled using default settings, making it easy to get started without additional configuration. However, if you require more advanced functionality or custom settings, you will need to build the bootloader yourself to tailor it to your specific needs. To do that, you need to:
1. Download and set up the [esp-idf]
2. Make desired changes using `idf.py menuconfig`
3. Re-build it
4. Use the built bootloader binary with `espflash flash --bootloader <FILE>` command 

## Partition Table

See more information in [partition-table] chapter.


[bootloaders]: https://github.com/esp-rs/espflash/tree/main/espflash/resources/bootloaders
[esp-idf]: https://github.com/espressif/esp-idf
[partition-table]: ../introduction/startup-flow.md