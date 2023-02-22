# Comparing `std` and `no_std`

Several factors must be considered when choosing between `std` ([esp-idf-hal]) and `no_std` (eg. [esp-hal]). As stated previously, each approach has its own unique set of advantages and disadvantages. While we can't decide for you, this section will hopefully allow you to make an educated decision.

[esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal
[esp-hal]: https://github.com/esp-rs/esp-hal

## Application Runtimes

In the case of applications (as opposed to libraries) the standard library provides a runtime that handles command-line arguments, setting up stack overflow protection, and spawning the main thread before an application's `main` function is invoked.

Applications targeting `no_std` will be responsible for initializing their own runtimes instead. Runtime initialization is generally handled by an external dependency, in our case the [riscv-rt] and [xtensa-lx-rt] libraries. You can refer to their READMEs and documentation for more information.

One advantage of not including the default runtime is that you're able to write applications at a lower level. This is possible because the applications will have been linked against the `core` crate instead of `std`, which makes no assumptions about the system it is running on. As such, it's possible to write applications like bootloaders, firmware, or even operating system kernels using the `no_std` approach.

[riscv-rt]: https://github.com/rust-embedded/riscv-rt
[xtensa-lx-rt]: https://github.com/esp-rs/xtensa-lx-rt

## `#![no_main]`

Another interesting property of `no_std` applications is that we cannot use Rust's default `main` function as our entry point. It makes certain assumptions that are not necessarily valid in an embedded context (for example, it expects that command-line arguments exist).

Because of this, you will often see the `#![no_main]` attribute used to instruct the Rust compiler not to use the default entry point. Runtime crates will provide an `#[entry]` attribute which can be used to mark a diverging function as the application's entry point instead. For example, a minimal application might look something like this:

```rust,ignore
#![no_std]
#![no_main]

use riscv_rt::entry;

#[entry]
fn main() -> ! {
    loop {}
}
```

## Panic Handlers

In addition to specifying the application's entry point, for `no_std` we must also define a panic handler. The default panic behaviour relies on `std`, as it prints to standard output.

You can define a panic handler manually using the `#[panic_handler]` attribute. Note that this function's signature _must_ match the example below.

```rust,ignore
#![no_std]

use core::panic::PanicInfo;

#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    // Your implementation goes here!
}
```

Alternatively, there are several external dependencies which define various panic handlers for us. Some possible choices are [panic-halt], [panic-semihosting], or [panic-never].

These can be used simply by installing the relevant dependency, and then importing the crate:

```rust,ignore
#![no_std]

use panic_halt as _;
```

Probably the most convenient panic handler for `no_std` on `esp-hal` is [esp-backtrace]

[panic-halt]: https://github.com/korken89/panic-halt
[panic-semihosting]: https://github.com/rust-embedded/cortex-m/tree/master/panic-semihosting
[panic-never]: https://github.com/japaric/panic-never
[esp-backtrace]: https://github.com/esp-rs/esp-backtrace
