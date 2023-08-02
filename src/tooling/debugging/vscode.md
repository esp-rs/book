# Debugging in Visual Studio Code

There is also a possibility to debug with graphical output directly in Visual Studio Code.

## ESP32

### Configuration

1. Connect an external JTAG adapter: [ESP-Prog][esp-prog] can be used.

|  ESP32 Pin  | JTAG Signal |
| :---------: | :---------: |
| MTDO/GPIO15 |     TDO     |
| MTDI/GPIO12 |     TDI     |
| MTCK/GPIO13 |     TCK     |
| MTMS/GPIO14 |     TMS     |
|     3V3     |    VJTAG    |
|     GND     |     GND     |

> ⚠️ **Note**: On Windows `USB Serial Converter A 0403 6010 00` driver should be WinUSB.

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

[esp-prog]: https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html
[cortex-debug]: https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug

## ESP32-C3

The availability of built-in JTAG interface depends on the ESP32-C3 revision:

- Revisions older than 3 **don't** a have built-in JTAG interface.
- Revisions 3 (and newer) **do** have a built-in JTAG interface, and you don't have to connect an external device to be able to debug.

To find your ESP32-C3 revision, run:

```shell
cargo espflash board-info
# or
espflash board-info
```

### Configuration

1. (**Only for revisions older than 3**) Connect an external JTAG adapter, [ESP-Prog][esp-prog] can be used.

| ESP32-C3 Pin | JTAG Signal |
| :----------: | :---------: |
|  MTDO/GPIO7  |     TDO     |
|  MTDI/GPIO5  |     TDI     |
|  MTCK/GPIO6  |     TCK     |
|  MTMS/GPIO4  |     TMS     |
|     3V3      |    VJTAG    |
|     GND      |     GND     |

> ⚠️**Note**: On Windows `USB Serial Converter A 0403 6010 00` driver should be WinUSB.

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
