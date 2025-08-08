# Application Development

This chapter covers essential topics for developing applications:

 - [Building a Custom ESP-IDF Bootloader](./bootloader.md)
 - [Using `esp-config`](./configuration.md)
 - [Logging](./logging.md)
 - [Allocating Memory](./alloc.md)
 - [Async Options](./async.md)
 - [Testing](./testing.md)

Before going into a detailed description of the various packages and concepts for writing an application, it might be useful for the user to have a superficial familiarity with the whole ecosystem, both in the `esp-hal` sense and in the broader Embedded Rust sense.

 ## `esp-hal` Ecosystem 

The first step in working with a project is to create it, the main way to do this is to use `esp-generate` Read more about it in the [section](../getting-started/tooling/esp-generate.md) dedicated to this tool
 
The alpha and omega that ties all work with Espressif chips in Rust is the `esp-hal` crate. Through it, the user will be able to perform basic initialization of the chip, as well as access drivers for the peripherals available on the chip. The full [`esp-hal` documentation] for selected chip will give unambiguous information about what peripherals are available to use.

Further, the user may want to use more advanced functionality of their chip. For example, network and connectivity. This part of the ecosystem is entirely the responsibility of `esp-radio`, which combines drivers for the communication protocols available on one or another of Espressif's products: `Wi-Fi`, `BLE`, `esp-now` and low-level `IEEE 802.15.4` for the lower layers of communication. More detailed information for each chip is available in the [`esp-radio` sub-repository].

For more advanced work with chip memory and to use collections from the `core` library that require heap allocation, the user is offered to use `esp-alloc`. A separate [chapter in the book](./alloc.md) is devoted to this.

## Embedded Rust Ecosystem Integration

The most popular Hardware Abstraction Layer in the Embedded Rust environment is [`embedded-hal`], which provides a number of traits for several peripherals that simplify and unify the user's work with them. `esp-hal` also implements these traits within its drivers. In addition, various traits from different crates used in the embedded Rust industry have also been implemented, such as [`rand_core`] traits for our RNG driver, as well as [`embedded-io`] traits, which are analogues of `std::io` traits for `no_std` applications.

[`esp-hal` documentation]: https://docs.espressif.com/projects/rust/esp-hal/latest/
[`esp-radio` sub-repository]: https://github.com/esp-rs/esp-hal/tree/main/esp-radio
[`embedded-hal`]: https://docs.rs/embedded-hal/latest/embedded_hal/index.html
[`rand_core`]: https://crates.io/crates/rand_core
[`embedded-io`]: https://crates.io/crates/embedded-io