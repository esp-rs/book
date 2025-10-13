# Over The Air Updates (OTA)

Over The Air Updates (OTA), is the ability to update an application _without_ the need of production flashing tools. OTA is heavily reliant on a bootloader to handle the switching, replacement and rollback of OTA images (firmware updates). For every bootloader we support, we have a bootloader support crate, as we only support the ESP-IDF bootloader right now we only have the [`esp-bootloader-esp-idf`] crate.

We have a [small OTA example] in the `esp-hal` repository. This example is basic, but shows the building blocks to get OTA functionality working. Be sure to check out the documentation, as this example also provides the instructions to create an OTA binary using `espflash`.


[`esp-bootloader-esp-idf`]: https://github.com/esp-rs/esp-hal/blob/main/esp-bootloader-esp-idf
[small OTA example]: https://github.com/esp-rs/esp-hal/tree/main/examples/ota
