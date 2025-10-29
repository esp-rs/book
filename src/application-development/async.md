# Async Options

> ⚠️ **Note**:  This section does not serve as an async tutorial or teaching material. For these purposes, visit [official async-book].

`esp-hal` provides blocking and async API for most of the supported drivers. Drivers are constructed in [`Blocking`] mode by default. To set up an async driver, they must be converted to an [`Async`] mode using the `into_async` method. For more information and to get started, check our `examples` in the [esp-hal package].

> ⚠️ **Note**: Our `Async` drivers are not `Send` because they register interrupts on the current core. Moving them to another core can cause issues. If user needs to send (move) a driver to another core, they should send the `Blocking` version, and then call `into_async` on the correct core to bind it correctly.


## Embassy

[Embassy] is an asynchronous (async) framework designed specifically for embedded Rust development; its [embassy-executor] crate provides an `async/await` executor which executes a fixed number of tasks, statically allocated at startup, though more can be added later. To spawn more tasks later, you may keep copies of the `Spawner`, for example by passing it as an argument to the initial tasks. For more information about `embassy` visit [Embassy book].

The [`esp-rtos`] crate provides integration between [`esp-hal`] and the [Embassy] asynchronous framework. It provides support for:

1. Interrupt-mode executor
2. Multicore-aware thread-mode embassy executor
3. Embassy time driver
4. Timer waiter queue

## ArielOS

[ArielOS] is an operating system for secure, memory-safe, low-power IoT. It builds on top of various projects from the Embedded Rust ecosystem, including `esp-hal`. ArielOS focuses on tight integration, adding missing OS features like a multicore scheduler, secure networking, portable drivers, and a unified build system. The result is a powerful alternative to C-based Real-Time Operating System (RTOS) solutions, but in pure Rust. It has great integrations with embassy, and can be used together with embassy in various ways.

## RTIC

[Real-Time Interrupt-driven Concurrency (RTIC)] is a community supported concurrency framework for building real-time systems. Real time tasks are not async, but "software" tasks are async. Currently, only ESP32-C3 and ESP32-C6 are supported.


<!-- TODO: change ArielOS to crates.io link when it's ready -->
[official async-book]: https://rust-lang.github.io/async-book/
[`Blocking`]: https://docs.espressif.com/projects/rust/esp-hal/1.0.0/esp32c6/esp_hal/struct.Blocking.html
[`Async`]:  https://docs.espressif.com/projects/rust/esp-hal/1.0.0/esp32c6/esp_hal/struct.Async.html
[Embassy]: https://embassy.dev
[embassy-executor]: https://crates.io/crates/embassy-executor
[`esp-rtos`]: https://crates.io/crates/esp-rtos
[`esp-hal`]: https://crates.io/crates/esp-hal
[Embassy book]: https://embassy.dev/book/
[esp-hal package]: https://github.com/esp-rs/esp-hal
[ArielOS]: https://github.com/ariel-os/ariel-os
[Real-Time Interrupt-driven Concurrency (RTIC)]: https://crates.io/crates/rtic
