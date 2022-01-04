# Ecosystem Overview

As it stands there are two approaches for using Rust on Espressif chips, `std` and `no_std`. Both approaches have their advantages and disadvantages, so you should choose one based on your project's needs. Below is an overview of the two approaches followed by a brief comparison between them.

The [esp-rs organization] on GitHub is home to a number of repositories related to running Rust on Espressif chips. Most of the required crates have their source code hosted here.

[esp-rs organization]: https://github.com/esp-rs/

## Using the Rust Standard Library (`std`)

Espressif provides a C-based development framework called [`esp-idf`] which has support for Espressif chips from the ESP32 and later. `esp-idf` provides a [newlib] environment with enough functionality to build the Rust standard library (`std`) on top of it, hence this approach. The supported `std` features are as follows:

- Threads
- Mutexes and other synchronization primitives
- Collections
- Random number generation
- Sockets

On top of `std` features, there is a [embedded-svc] implementation for `esp-idf` which adds extra support for services/modules not available in the standard library, including:

- Wi-Fi management
- NVS (non-volatile storage)
- Networking services like `httpd` and `ping`

In general, this approach should feel quite similar to developing for most normal PC environments.

[`esp-idf`]: https://github.com/espressif/esp-idf
[newlib]: https://sourceware.org/newlib/
[embedded-svc]: https://github.com/esp-rs/embedded-svc

### Chip Support

Chip support for `std` requires two things, LLVM/Clang support and support in `esp-idf`. Refer to the table below to see if your chip is supported.

|   Chip   | Supported? |
| :------: | :--------: |
|  ESP32   |     ✅     |
| ESP32-C3 |     ✅     |
| ESP32-S2 |     ✅     |
| ESP32-S3 |     ✅     |
| ESP32-H2 |  planned   |
| ESP8266  |     ❌     |

### Relevant `esp-rs` crates

| Repository            | Description                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| [esp-rs/esp-idf-hal]  | An implementation of the `embedded-hal` traits using the `esp-idf` framework. Provides the Rust `std` library.|
| [esp-rs/embedded-svc] | Abstraction traits for embedded services.                                                                     |
| [esp-rs/esp-idf-svc]  | An implementation of [embedded-svc] using `esp-idf` drivers.                                                  |
| [esp-rs/esp-idf-sys]  | Rust bindings to the `esp-idf` development framework. Gives raw (`unsafe`) access to drivers, Wi-Fi and more. |

[esp-rs/embedded-svc]: https://github.com/esp-rs/embedded-svc
[esp-rs/esp-idf-svc]: https://github.com/esp-rs/esp-idf-svc
[esp-rs/esp-idf-sys]: https://github.com/esp-rs/esp-idf-sys
[esp-rs/esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal

## Bare Metal (`no_std`)

Using `no_std` may be more familiar to embedded Rust developers. It does not use `std` (the Rust standard library) but instead uses a subset, the `core` library. The [official Rust embedded book] has a [great section on this].

It's important to note that in general a `no_std` crate can always compile in `std` environment but the inverse is not true, therefore when creating crates it's worth keeping in mind if it needs to the standard library to function.

The main focus of the `no_std` ESP support has been around the ESP32 and to a lesser extent the ESP8266. As [SVDs are released] it will be possible to create hardware abstraction layers (HAL) for newer chips.

While `esp32-hal` has great support for a lot of the onboard peripherals, Wi-Fi and Bluetooth are **not** currently supported in a `no_std` enviroment. [esp-wifi] was created to explore getting it working, but it is not yet functional.

[official rust embedded book]: https://docs.rust-embedded.org/
[great section on this]: https://docs.rust-embedded.org/book/intro/no-std.html
[svds are released]: https://github.com/espressif/svd
[esp-wifi]: https://github.com/esp-rs/esp32-wifi

### Chip Support

Chip support for `no_std` requires LLVM/Clang support just like for `std`. However, this has no dependency on `esp-idf`. In addition to compiler support, it's necessary to have peripheral access crates (PAC) and hardware abstraction layers (HAL) for your desired chip.

Refer to the table below to see if your chip is supported.

|   Chip   |   PAC   |   HAL   |
| :------: | :-----: | :-----: |
|  ESP32   |   ✅    |   ✅    |
| ESP32-C3 |   ✅    | planned |
| ESP32-S2 |   ⚠️    | planned |
| ESP32-S3 |   ⚠️    | planned |
| ESP32-H2 | planned | planned |
| ESP8266  |   ✅    |   ✅    |

**Note:** _items marked with ⚠️ have repositories, but have not yet been published to [crates.io]._

[crates.io]: https://crates.io/

### Relevant `esp-rs` Crates

| Repository           | Description                                                                                      |
| -------------------- | ------------------------------------------------------------------------------------------------ |
| [esp-rs/esp32-hal]   | An implementation of the `embedded-hal` traits and more for the ESP32 using bare metal (no_std). |
| [esp-rs/esp32]       | A PAC crate for the ESP32.                                                                       |
| [esp-rs/esp32c3]     | A PAC crate for the ESP32-C3.                                                                    |
| [esp-rs/esp32s2]     | A PAC crate for the ESP32-S2.                                                                    |
| [esp-rs/esp32s3]     | A PAC crate for the ESP32-S3.                                                                    |
| [esp-rs/esp8266-hal] | An implementation of the `embedded-hal` traits and more for the ESP8266.                         |
| [esp-rs/esp8266]     | A PAC crate for the ESP8266.                                                                     |

[esp-rs/esp32-hal]: https://github.com/esp-rs/esp32-hal
[esp-rs/esp32]: https://github.com/esp-rs/esp32
[esp-rs/esp32c3]: https://github.com/esp-rs/esp32c3
[esp-rs/esp32s2]: https://github.com/esp-rs/esp32s2
[esp-rs/esp32s3]: https://github.com/esp-rs/esp32s3
[esp-rs/esp8266-hal]: https://github.com/esp-rs/esp8266-hal
[esp-rs/esp8266]: https://github.com/esp-rs/esp8266

## Comparing `std` and `no_std`

There are a number of factors which must be considered when choosing between `std` (esp-idf-hal) and `no_std` (esp32-hal). As stated earlier, each approach has its own unique set of advantages and disadvantages. While we can't decide for you, this section will hopefully allow you to make an educated decision.

At present, there are unfortunately certain technical restrictions which may dictate your choice; we hope to have these issues resolved soon. Currently you _must_ use the `std` approach if you require any of the following:

- Use of Wi-Fi or Bluetooth
- The ability to target any chip other than the ESP32 or the ESP8266

### Application Runtimes

In the case of applications (as opposed to libraries) the standard library provides a runtime which handles setting up stack overflow protection, spawning the main thread before an application's `main` function is invoked, and handling of command-line arguments.

Applications targeting `no_std` will be responsible for initializing their own runtimes instead. Runtime initialization is generally handled by an external dependency, in our case the [riscv-rt] and [xtensa-lx-rt] libraries. You can refer to their READMEs and documentation for more information.

One advantage of not including the default runtime is that you're able to write applications at a lower level. This is possible because the applications will have been linked again the `core` crate instead of `std`, which makes no assumptions about the system it is running on. As such, it's possible to write applications like bootloaders, firmware, or even operating system kernels using the `no_std` approach.

[riscv-rt]: https://github.com/rust-embedded/riscv-rt
[xtensa-lx-rt]: https://github.com/esp-rs/xtensa-lx-rt

### `#![no_main]`

Another interesting property of `no_std` applications is that we cannot use Rust's default `main` function as our entry point. It makes certain assumptions which are not neccessarily valid in an embedded context (for example, it expects that command-line arguments exist).

Because of this, you will often see the `#![no_main]` attribute used to instruct the Rust compiler not to use the default entry point. Runtime crates will provide an `#[entry]` attribute which can be used to mark a diverging function as the application's entry point instead. For example a minimal application might look something like this:

```rust,ignore
#![no_std]
#![no_main]

use riscv_rt::entry;

#[entry]
fn main() -> ! {
    loop {}
}
```

### Panic Handlers

In addition to specifying the application's entry point, for `no_std` we must also define a panic handler. The default panic behaviour relies on `std`, as it prints to standard output.

You are able to define a panic handler manually using the `#[panic_handler]` attribute. Note that this function's signature _must_ match the one below.

```rust,ignore
#![no_std]

use core::panic::PanicInfo;

#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    // Your implementation goes here!
}
```

Alternatively, there are a number of external dependencies which define various panic handlers for us. Some possible choices are [panic-halt], [panic-semihosting], or [panic-never].

These can be used simply by importing the relevant crate:

```rust,ignore
#![no_std]

use panic_halt as _;
```

[panic-halt]: https://github.com/korken89/panic-halt
[panic-semihosting]: https://github.com/rust-embedded/cortex-m/tree/master/panic-semihosting
[panic-never]: https://github.com/japaric/panic-never
