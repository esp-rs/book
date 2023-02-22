# Wokwi

[Wokwi] is an online simulator that supports simulating Rust projects (both `std` and `no_std`) in ESP Chips,
see [wokwi.com/rust] for a list of examples and a way to start new projects.

Wokwi offers Wi-Fi simulation, Virtual Logic Analyzer, and [GDB debugging] among many other features, see
[Wokwi documentation] for more details. For ESP chips, there is a [table of simulation features that are currently supported].

## Using Wokwi for VS Code extension
Wokwi offers a VS Code extension that allows users to simulate their project directly from your code editor by only adding a few files. For more information, see [Wokwi documentation][wokwi-vscode].
You can also debug your code using the VS Code debugger, see [Debugging your code].

When using any of the [templates], there is a prompt that generates the required files to use Wokwi VS Code extension.

## Using wokwi-server

[wokwi-server] is a CLI tool for launching a Wokwi simulation of your project. I.e., it allows you
to build a project on your machine, or in a container, and simulate the resulting binary.

[wokwi-server] also allows simulating your resulting binary on other Wokwi projects, with more hardware parts other than the chip itself. See the [corresponding section of the wokwi-server README] for detailed instructions.

## Custom chips
Wokwi allows generating custom chips that let you program the behavior of a component not supported in Wokwi. For more details, see the official [Wokwi documentation][wokwi-custom-chip]


[Wokwi]: https://wokwi.com/
[wokwi.com/rust]: https://wokwi.com/rust
[GDB debugging]: https://docs.wokwi.com/gdb-debugging
[Wokwi documentation]: https://docs.wokwi.com/
[table of simulation features that are currently supported]: https://docs.wokwi.com/guides/esp32#simulation-features
[wokwi-server]: https://github.com/MabezDev/wokwi-server
[corresponding section of the wokwi-server Readme]: https://github.com/MabezDev/wokwi-server#simulating-your-binary-on-a-custom-wokwi-project
[wokwi-vscode]: https://docs.wokwi.com/vscode/getting-started
[Debugging your code]: https://docs.wokwi.com/vscode/debugging
[templates]: ./../../writing-your-own-application/generate-project/index.md
[wokwi-custom-chip]: https://docs.wokwi.com/chips-api/getting-started
