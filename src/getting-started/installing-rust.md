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

[rustup]: https://rustup.rs/

## RISC-V (ESP32-C3)

The `RISC-V` architecture has support in the mainline Rust compiler so setup is relatively simple, all we must do is add the appropriate compilation target.

There are two suitable targets for this chip:

- For bare-metal (`no_std`) applications, use `riscv32imc-unknown-none-elf`
- For applications which require `std`, use `riscv32imc-esp-espidf`

The compilation targets can be installed by running:

```bash
$ rustup target add riscv32imc-unknown-none-elf
$ rustup target add riscv32imc-esp-espidf
```

At this point you are ready to build applications for the ESP32-C3.

[rustup]: https://rustup.rs/
[esp-idf]: https://github.com/espressif/esp-idf

## Xtensa (ESP32, ESP32-S2)

Because there is no `Xtensa` support in the mainline Rust compiler you must use the [esp-rs/rust] fork instead. There are a few options available for installing this compiler fork.

### Using a Pre-Built Release

Pre-built releases are available for a number of platforms on GitHub under the [esp-rs/rust-build] repository. The following operating systems and architectures are currently supported:

- macOS (`x86_64`, `aarch64`)
- Windows (`x86_64`)
- Linux (`x86_64`)

The aforementioned repository also contains Bash and PowerShell scripts to automate the installation process.

On _macOS_ or _Linux_:

```bash
$ curl -LO https://raw.githubusercontent.com/esp-rs/rust-build/main/install-rust-toolchain.sh
$ chmod +x install-rust-toolchain.sh
$ ./install-rust-toolchain.sh
```

On _Windows_:

```powershell
PS> Invoke-WebRequest https://raw.githubusercontent.com/esp-rs/rust-build/main/Install-RustToolchain.ps1
PS> ./Install-RustToolchain.ps1
```

To confirm the `esp` toolchain has been installed:

```bash
$ rustup toolchain list
stable-x86_64-apple-darwin
nightly-x86_64-apple-darwin (default)
esp
```

### Building From Source

You can also build the Rust compiler with `Xtensa` support from source. This process is computationally expensive and can take one or more hours to complete depending on your system. It is recommended that you have _at least_ 6GB of RAM and 25GB+ of available storage space.

```bash
$ git clone -b esp https://github.com/esp-rs/rust
$ cd rust
$ ./configure --experimental-targets=Xtensa
$ ./x.py build --stage 2
```

Note that you should _not_ rename the `rust` directory to avoid issues while building.

Once the build has completed, you can link the toolchain using rustup (your architecture/operating system may be different):

```bash
$ rustup toolchain link esp $PWD/build/x86_64-apple-darwin/stage2
```

To confirm the `esp` toolchain has been installed:

```bash
$ rustup toolchain list
stable-x86_64-apple-darwin
nightly-x86_64-apple-darwin (default)
esp
```

To view the installed `Xtensa` targets:

```bash
$ rustup default esp
$ rustc --print target-list | grep xtensa
xtensa-esp32-espidf
xtensa-esp32-none-elf
xtensa-esp32s2-espidf
xtensa-esp32s2-none-elf
xtensa-esp8266-none-elf
xtensa-none-elf
```

[esp-rs/rust]: https://github.com/esp-rs/rust
[esp-rs/rust-build]: https://github.com/esp-rs/rust-build
