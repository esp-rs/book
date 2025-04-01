# Async

This subchapter does not serve as an `async` tutorial or teaching material. For these purposes visit [official async-book]. 

The [esp-hal-embassy] crate provides integration between the [esp-hal] and the [Embassy] asynchronous framework. It provides support for:

1. Interrupt-mode executor
2. Multicore-aware thread-mode embassy executor
3. Embassy time driver
4. Timer waiter queue

`esp-hal` provides `blocking` and `async` API for most of the supported drivers. For more information and to get started check our `examples` in the [esp-hal-package].

## Embassy

[Embassy] is an asynchronous (async) framework designed specifically for embedded Rust development which consists of several crates that you can use together or independently. One of the projects is [embassy-executor]. The `embassy-executor` is an `async/await` executor that generally executes a fixed number of tasks, allocated at startup, though more can be added later. For more information about `embassy` visit [Embassy book].


## RTIC

[Real-Time Interrupt-driven Concurrency (RTIC)] is a concurrency framework for building real-time systems. Currently, only the ESP32-C3 is supported.


[official async-book]: https://rust-lang.github.io/async-book/
[Embassy]: https://embassy.dev
[embassy-executor]: https://crates.io/crates/embassy-executor
[esp-hal-embassy]: https://crates.io/crates/esp-hal-embassy
[esp-hal]: https://crates.io/crates/esp-hal
[Embassy book]: https://embassy.dev/book/
[esp-hal-package]: https://github.com/esp-rs/esp-hal
[Real-Time Interrupt-driven Concurrency (RTIC)]: https://crates.io/crates/rtic
