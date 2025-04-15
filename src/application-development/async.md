# Async

This section does not serve as an async tutorial or teaching material. For these purposes, visit [official async-book]. 

The [`esp-hal-embassy`][esp-hal-embassy] crate provides integration between the [`esp-hal`][esp-hal] and the [Embassy] asynchronous framework. It provides support for:

1. Interrupt-mode executor
2. Multicore-aware thread-mode embassy executor
3. Embassy time driver
4. Timer waiter queue

`esp-hal` provides blocking and async API for most of the supported drivers. For more information and to get started, check our `examples` in the [esp-hal-package].

## Embassy

[Embassy] is an asynchronous (async) framework designed specifically for embedded Rust development; its [embassy-executor] crate provides an `async/await` executor which executes a fixed number of tasks, statically allocated at startup, though more can be added later. To spawn more tasks later, you may keep copies of the `Spawner` (it is `Copy`), for example by passing it as an argument to the initial tasks. For more information about `embassy` visit [Embassy book].


## RTIC

[Real-Time Interrupt-driven Concurrency (RTIC)] is a concurrency framework for building real-time systems. Currently, the ESP32-C3 and the ESP32-C6 are supported.


[official async-book]: https://rust-lang.github.io/async-book/
[Embassy]: https://embassy.dev
[embassy-executor]: https://crates.io/crates/embassy-executor
[esp-hal-embassy]: https://crates.io/crates/esp-hal-embassy
[esp-hal]: https://crates.io/crates/esp-hal
[Embassy book]: https://embassy.dev/book/
[esp-hal-package]: https://github.com/esp-rs/esp-hal
[Real-Time Interrupt-driven Concurrency (RTIC)]: https://crates.io/crates/rtic
