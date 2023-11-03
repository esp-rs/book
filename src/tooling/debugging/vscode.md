# `cortex-debug` VS Code Extension

The [`cortex-debug`][cortex-debug] VS Code extension also allows debugging Espressif products. Products containing the [`USB-JTAG-SERIAL` peripheral][usb-jtag-serial] do'nt require any external hardware, but other products
may use an external JTAG debugger, like [ESP-Prog][esp-prog].


[cortex-debug]: https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug
[usb-jtag-serial]: index.md#usb-jtag-serial-peripheral
[esp-prog]: https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html

### Configuration

1. If required, connect the external JTAG adapter.
   1. See Configure Other JTAG Interfaces section of ESP-IDF Programming Guide. Eg: [Section for ESP32][jtag-interfaces-esp32]
> ⚠️ **Note**: On Windows, `USB Serial Converter A 0403 6010 00` driver should be WinUSB.
2. Set up VSCode
   1. Install [Cortex-Debug][cortex-debug] extension for VS Code.
   2. Create the `.vscode/launch.json` file in the project tree you want to debug.
   3. Update `executable`, `svdFile`, `serverpath` paths, and `toolchainPrefix` fields.

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      // more info at: https://github.com/Marus/cortex-debug/blob/master/package.json
      "name": "Attach",
      "type": "cortex-debug",
      "request": "attach", // launch will fail when attempting to download the app into the target
      "cwd": "${workspaceRoot}",
      "executable": "target/xtensa-esp32-none-elf/debug/.....", //!MODIFY
      "servertype": "openocd",
      "interface": "jtag",
      "toolchainPrefix": "xtensa-esp32-elf", //!MODIFY
      "openOCDPreConfigLaunchCommands": ["set ESP_RTOS none"],
      "serverpath": "C:/Espressif/tools/openocd-esp32/v0.11.0-esp32-20220411/openocd-esp32/bin/openocd.exe", //!MODIFY
      "configFiles": ["board/esp32-wrover-kit-3.3v.cfg"], //!MODIFY
      "overrideAttachCommands": [
        "set remote hardware-watchpoint-limit 2",
        "mon halt",
        "flushregs"
      ],
      "overrideRestartCommands": ["mon reset halt", "flushregs", "c"]
    }
  ]
}
```

[jtag-interfaces-esp32]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/configure-other-jtag.html

