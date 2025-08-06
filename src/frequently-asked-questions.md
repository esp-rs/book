# Frequently Asked Questions

This chapter addresses common questions and challenges that developers may encounter when working with Rust on ESP chips. Whether you're setting up your development environment, optimizing your code, or looking to simulate your projects, you'll find practical solutions and best practices here.

## Editor/IDE

When using [`esp-generate`][esp-generate], you can automatically configure recommended settings and extensions for VS Code, Helix, and Zed editors during the generation process.

[esp-generate]: ./getting-started/tooling/esp-generate.md

## Size and Memory Optimizations

### Optimizing Binary Size

- Cargo provides some default profiles; we recommend using the [`release` profile][release-profile] as it optimizes and removes debug symbols.
- Cargo allows different [profile settings][profile-settings-cargo], which can make a difference in the resulting size of the artifact.
  - See [Optimizations: the speed size tradeoff][embedded-book-tradeoffs] of The Embedded Rust Book.
- Be careful when using external dependencies, as they can increase the size of your resulting artifact.
- Filter log messages if they are not going to be useful or read.
- More suggestions can be found in the [min-sized-rust][min-sized-rust] repository.

Additionally, the [Embassy documentation][embassy-documentation] contains some suggestions with regards to binary sizes in the [Frequently Asked Questions][frequently-asked-questions] section.

[embedded-book-tradeoffs]: https://docs.rust-embedded.org/book/unsorted/speed-vs-size.html
[release-profile]: https://doc.rust-lang.org/cargo/reference/profiles.html#release
[profile-settings-cargo]: https://doc.rust-lang.org/cargo/reference/profiles.html#profile-settings
[min-sized-rust]: https://github.com/johnthagen/min-sized-rust
[embassy-documentation]: https://embassy.dev/book
[frequently-asked-questions]: https://embassy.dev/book/#_frequently_asked_questions

### Optimizing Memory Usage

We will, again, defer to the [Embassy Documentation][embassy-documentation], specifically the [How can I measure resource usage (CPU, RAM, etc.)?][measure-resources] section.

[measure-resources]: https://embassy.dev/book/#_how_can_i_measure_resource_usage_cpu_ram_etc

## Using Crates from Git

The [Cargo Book][cargo-book] and the [Embassy Documentation][embassy-documentation] both contain information on how to specify dependencies from Git repositories:

- [Specifying dependencies from `git` repositories][dependencies-from-git]
- [The `[patch]` section][patch-section]
- [How do I switch to the `main` branch][switch-to-main-branch]

[cargo-book]: https://doc.rust-lang.org/cargo/
[dependencies-from-git]: https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#specifying-dependencies-from-git-repositories
[patch-section]: https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html#the-patch-section
[switch-to-main-branch]: https://embassy.dev/book/#_how_do_i_switch_to_the_main_branch

## Simulating Projects

Simulating projects can be handy. It allows users to test projects using CI, try projects without having hardware available, and many other scenarios.

In this section, we will discuss currently available simulation tools.

### Wokwi

[Wokwi][wokwi] is an online simulator that supports simulating Rust projects (`no_std`) in Espressif Chips.
See [wokwi.com/rust][wokwi-rust] for a list of examples and a way to start new projects.

Wokwi offers Wi-Fi simulation, Virtual Logic Analyzer, and [GDB debugging][gdb-debugging] among many other features; see
[Wokwi documentation][wokwi-documentation] for more details. For ESP chips, there is a table of [simulation features][wokwi-simulation-features] that are currently supported.

Refer to the table below for chip support status in Wokwi:

|              | **[Wokwi][wokwi-simulation-features]** |
| :----------: | :------------------------------------: |
|  **ESP32**   |                   ✅                    |
| **ESP32-C2** |                   ❌                    |
| **ESP32-C3** |                   ✅                    |
| **ESP32-C6** |                   ✅                    |
| **ESP32-H2** |                   ✅                    |
| **ESP32-S2** |                   ✅                    |
| **ESP32-S3** |                   ✅                    |

[wokwi]: https://wokwi.com/
[wokwi-rust]: https://wokwi.com/rust
[gdb-debugging]: https://docs.wokwi.com/gdb-debugging
[wokwi-documentation]: https://docs.wokwi.com/
[wokwi-simulation-features]: https://docs.wokwi.com/guides/esp32#simulation-features

#### Using Wokwi for VS Code extension

Wokwi offers a VS Code extension that allows you to simulate a project directly in the code editor by only adding a few files.
For more information, see [Wokwi documentation][wokwi-vscode].
You can also debug your code using the VS Code debugger; see [Debugging your code][wokwi-debugging].

When using [`esp-generate`][esp-generate], there is an option that generates the required files to use Wokwi VS Code extension.

[wokwi-vscode]: https://docs.wokwi.com/vscode/getting-started
[wokwi-debugging]: https://docs.wokwi.com/vscode/debugging
[esp-generate]: ./getting-started/tooling/esp-generate.md

#### Using `wokwi-server`

[`wokwi-server`][wokwi-server] is a CLI tool for launching a Wokwi simulation of your project. It allows you
to build a project on your machine, or in a container, and simulate the resulting binary.

[`wokwi-server`][wokwi-server] also allows simulating your resulting binary on other Wokwi projects, with more hardware parts other than the chip itself. See the corresponding [section of the `wokwi-server`][wokwi-server-custom] README for detailed instructions.

[wokwi-server]: https://github.com/MabezDev/wokwi-server
[wokwi-server-custom]: https://github.com/MabezDev/wokwi-server#simulating-your-binary-on-a-custom-wokwi-project

#### Custom Chips

Wokwi allows generating custom chips that let you program the behavior of a component not supported in Wokwi. For more details, see the official [Wokwi documentation][wokwi-custom-chip].

Custom chips can also be written in Rust! See [Wokwi Custom Chip API][rust-chip-api] for more information. For example, custom [inverter chip][custom-chip-example] in Rust.

[wokwi-custom-chip]: https://docs.wokwi.com/chips-api/getting-started
[rust-chip-api]: https://github.com/wokwi/wokwi_chip_ll
[custom-chip-example]: https://github.com/wokwi/rust_chip_inverter
