# Installing Rust

In order to develop for ESP devices using Rust you must first install the Rust compiler along with the appropriate toolchain and target(s). Depending on your device it may be one of two architectures, each requiring different setup.

If you have not yet installed Rust on your system, you can do so easily using [rustup]. For _macOS_ and _Linux_ it can be installed by runing the following command:

```bash
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

For installation on Windows or alternative installation methods, please refer to the instructions on the [rustup] website.

With Rust installed we next need to ensure that the `nightly` toolchain is installed and set as the default:

```bash
$ rustup toolchain install nightly
$ rustup default nightly
```

You can read more about toolchains in the [rustup book].

[rustup]: https://rustup.rs/
[rustup book]: https://rust-lang.github.io/rustup/concepts/toolchains.html

## Prerequisites

### git

`git` must be installed on your system in order to clone repositories. This should be available via your system's package manager, or for Windows users [Git for Windows] can be used.

[git for windows]: https://gitforwindows.org/

### Visual Studio Build Tools

If you are running Windows as your host operating system, you must install the Visual Studio Build Tools, which can be downloaded from the [Microsoft website].

[microsoft website]: https://visualstudio.microsoft.com/downloads/

### Xtensa Toolchain

If you are developing for an Xtensa chip (_ESP32_, _ESP32-S2_, _ESP32-S3_) you must also install the appropriate Xtensa toolchain. Pre-built toolchains can be downloaded from the [crosstool-NG] repository for the most common operating systems and architectures.

Ensure that you have downloaded the required toolchain and added its directory to your `PATH` environment variable prior to building your application.

[crosstool-ng]: https://github.com/espressif/crosstool-NG

## RISC-V (ESP32-C3)

The `RISC-V` architecture has support in the mainline Rust compiler so setup is relatively simple, all we must do is add the appropriate compilation target.

There are two suitable targets for this chip:

- For bare-metal (`no_std`) applications, use `riscv32imc-unknown-none-elf`
- For applications which require `std`, use `riscv32imc-esp-espidf`

The bare-metal target can be installed by running:

```bash
$ rustup target add riscv32imc-unknown-none-elf
```

The standard library target (`riscv32imc-esp-espidf`) is currently Tier 3, and does not have prebuilt objects distributed through `rustup`, therefore the `-Z build-std` unstable cargo feature is required within your project. See an example usage in [rust-esp32-std-mini](https://github.com/ivmarkov/rust-esp32-std-mini/blob/5fe3a5d75b16c6cee63e4c36b87f936744151494/.cargo/config.toml#L25-L26).

At this point you are ready to build applications for the ESP32-C3.

[rustup]: https://rustup.rs/
[esp-idf]: https://github.com/espressif/esp-idf

## Xtensa (ESP32, ESP32-S2, ESP32-S3)

Because there is no `Xtensa` support in the mainline Rust compiler you must use the [esp-rs/rust] fork instead. There are a few options available for installing this compiler fork.

The forked compiler can coexist with the standard Rust compiler, so it is possible to have both installed on your system. The forked compiler is invoked when using the `esp` channel instead of the defaults, `stable` or `nightly`.

[esp-rs/rust]: https://github.com/esp-rs/rust

### Using a Pre-Built Release

Pre-built releases are available for a number of platforms on GitHub under the [esp-rs/rust-build] repository. The following operating systems and architectures are currently supported:

- macOS (`x86_64`, `aarch64`)
- Windows (`x86_64`)
- Linux (`x86_64`)

The aforementioned repository also contains Bash and PowerShell scripts to automate the installation process.

#### macOS and Linux

```bash
$ curl -LO https://raw.githubusercontent.com/esp-rs/rust-build/main/install-rust-toolchain.sh
$ chmod +x install-rust-toolchain.sh
$ ./install-rust-toolchain.sh
```

#### Windows

With GUI installer: [https://github.com/espressif/idf-installer/releases]

With PowerShell:

```powershell
PS> Invoke-WebRequest https://raw.githubusercontent.com/esp-rs/rust-build/main/Install-RustToolchain.ps1 -OutFile Install-RustToolchain.ps1
PS> Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process -Force
PS> ./Install-RustToolchain.ps1
```

To confirm the `esp` toolchain has been installed:

```bash
$ rustup toolchain list
stable-x86_64-apple-darwin
nightly-x86_64-apple-darwin (default)
esp
```

[esp-rs/rust-build]: https://github.com/esp-rs/rust-build
[https://github.com/espressif/idf-installer/releases]: https://github.com/espressif/idf-installer/releases

### Building From Source

You can also build the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It is recommended that you have _at least_ 6GB of RAM and 25GB+ of available storage space.

To check out the repository and build the compiler:

```bash
$ git clone https://github.com/esp-rs/rust
$ cd rust
$ ./configure --experimental-targets=Xtensa
$ ./x.py build --stage 2
```

Note that you should _not_ rename the `rust` directory to avoid issues while building.

Once the build has completed, you can link the toolchain using rustup (your architecture/operating system may be different):

```bash
$ rustup toolchain link esp $PWD/build/x86_64-apple-darwin/stage2
```

Once the compiler fork has been installed using one of the above methods, to confirm the `esp` toolchain has been installed:

```bash
$ rustup toolchain list
stable-x86_64-apple-darwin
nightly-x86_64-apple-darwin (default)
esp
```

To view the installed `Xtensa` targets:

```bash
$ rustc +esp --print target-list | grep xtensa
xtensa-esp32-espidf
xtensa-esp32-none-elf
xtensa-esp32s2-espidf
xtensa-esp32s2-none-elf
xtensa-esp8266-none-elf
xtensa-none-elf
```

### Using Containers

As an alternative to installing the compiler fork to your local system directly, it's also possible to run it inside of a container.

A number of container runtimes are available, and which should be used depends on your operating system. Some of the popular options are:

- [Docker] (non-commerial use only without a license)
- [Podman]
- [Lima]

Espressif provides the [idf-rust] container image which contains [esp-idf] and the pre-built Rust compiler fork.

[docker]: https://www.docker.com/
[podman]: https://podman.io/
[lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust
