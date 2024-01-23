# Debugging

Debugging Rust applications is also possible using different tools that will be covered in this chapter.

Refer to the table below to see which chip is supported in every debugging method:

|              | **probe-rs** | **OpenOCD** |
| :----------: | :----------: | :---------: |
|  **ESP32**   |      ⏳       |      ✅      |
| **ESP32-C2** |      ✅       |      ✅      |
| **ESP32-C3** |      ✅       |      ✅      |
| **ESP32-C6** |      ✅       |      ✅      |
| **ESP32-H2** |      ✅       |      ✅      |
| **ESP32-S2** |      ✅       |      ✅      |
| **ESP32-S3** |      ✅       |      ✅      |

## `USB-JTAG-SERIAL` Peripheral

Some of our recent products contain the `USB-JTAG-SERIAL` peripheral that allows for debugging without any external hardware debugger. More info on configuring the interface can be found in the official documentation for the chips that support this peripheral:
- [ESP32-C3][esp32c3-docs]
    - The availability of built-in JTAG interface depends on the ESP32-C3 revision:
      - Revisions older than 0.3 **don't** have a built-in JTAG interface.
      - Revisions 0.3 (and newer) **do** have a built-in JTAG interface, and you don't have to connect an external device to be able to debug.

    To find your ESP32-C3 revision, run:
    ```shell
    cargo espflash board-info
    # or
    espflash board-info
    ```
- [ESP32-C6][esp32c6-docs]
- [ESP32-H2][esp32h2-docs]
- [ESP32-S3][esp32s3-docs]

[esp32c3-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32c6-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32h2-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32h2/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32s3-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/jtag-debugging/configure-builtin-jtag.html

