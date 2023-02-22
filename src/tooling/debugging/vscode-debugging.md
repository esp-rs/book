# Debugging in Visual Studio Code

There is also a possibility to debug with graphical output directly in Visual Studio Code.

## ESP32

### Hardware Setup

ESP32 doesn't have a built-in JTAG interface so you have to connect an external JTAG adapter to the ESP32 board, for example, [ESP-Prog](https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html) can be used.

|  ESP32 Pin  | JTAG Signal |
| :---------: | :---------: |
| MTDO/GPIO15 |     TDO     |
| MTDI/GPIO12 |     TDI     |
| MTCK/GPIO13 |     TCK     |
| MTMS/GPIO14 |     TMS     |
|     3V3     |    VJTAG    |
|     GND     |     GND     |

**Note**: On Windows `USB Serial Converter A 0403 6010 00` driver should be WinUSB.

## Set up VSCode

1. Install [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) extension for VScode.
2. Create the `.vscode/launch.json` file in the project tree you want to debug. [This](https://github.com/esp-rs/esp32-hal/blob/master/.vscode/launch.json) can be used as a template file.
3. Update **executable**, **svdFile**, **serverpath** paths, and **toolchainPrefix** field.

```jsonc
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
      "request": "attach", // attach instead of launch, because otherwise flash write is attempted, but fails
      "cwd": "${workspaceRoot}",
      "executable": "target/xtensa-esp32-none-elf/debug/.....",
      "servertype": "openocd",
      "interface": "jtag",
      "svdFile": "../../esp-pacs/esp32/svd/esp32.svd",
      "toolchainPrefix": "xtensa-esp32-elf",
      "openOCDPreConfigLaunchCommands": ["set ESP_RTOS none"],
      "serverpath": "C:/Espressif/tools/openocd-esp32/v0.11.0-esp32-20220411/openocd-esp32/bin/openocd.exe",
      "configFiles": ["board/esp32-wrover-kit-3.3v.cfg"],
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

## ESP32-C3

Older versions with **revision < 3** **don't** have built-in JTAG interface.

ESP32-C3 with **revision 3** **does** have a built-in JTAG interface and you don't have to connect an external device to be able to debug. To get the chip revision, run the `cargo espflash board-info` command.

### Hardware Setup

If your ESP32-C3's revision is lesser than 3, follow these instructions, if you have revision 3 you can jump to the [**Set up VSCode**](#set-up-vscode-1) step.

ESP32-C3 **revision 1** and **revision 2** don't have a built-in JTAG interface so you have to connect an external JTAG adapter to the ESP32-C3 board, for example, [ESP-Prog](https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html) can be used.

| ESP32-C3 Pin | JTAG Signal |
| :----------: | :---------: |
|  MTDO/GPIO7  |     TDO     |
|  MTDI/GPIO5  |     TDI     |
|  MTCK/GPIO6  |     TCK     |
|  MTMS/GPIO4  |     TMS     |
|     3V3      |    VJTAG    |
|     GND      |     GND     |

**Note**: On Windows `USB Serial Converter A 0403 6010 00` driver should be WinUSB.

### Set up VSCode

1. Install [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) extension for VScode.
2. Create the `.vscode/launch.json` file in the project tree you want to debug. [This](https://github.com/esp-rs/esp32-hal/blob/master/.vscode/launch.json) can be used as a template file.
3. Update **executable**, **svdFile**, **serverpath** paths, and **toolchainPrefix** field.

```jsonc
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
      "request": "attach", // attach instead of launch, because otherwise flash write is attempted, but fails
      "cwd": "${workspaceRoot}",
      "executable": "target/riscv32imc-unknown-none-elf/debug/examples/usb_serial_jtag", //
      "servertype": "openocd",
      "interface": "jtag",
      "svdFile": "../../esp-pacs/esp32c3/svd/esp32c3.svd",
      "toolchainPrefix": "riscv32-esp-elf",
      "openOCDPreConfigLaunchCommands": ["set ESP_RTOS none"],
      "serverpath": "C:/Espressif/tools/openocd-esp32/v0.11.0-esp32-20220411/openocd-esp32/bin/openocd.exe",
      "configFiles": ["board/esp32c3-builtin.cfg"],
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
