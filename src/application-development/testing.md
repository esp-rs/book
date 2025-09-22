# Testing

Testing is an integral part of application development. Using the example of how we do it at `esp-hal`, you can write and run tests for your application that run on the hardware itself.

## Host Testing

Where possible, and where it makes sense, you should try to test as much as possible on your host machine, not on the target device. It's easier to test in CI, faster, and won't waste flash write cycles on your device. Tests that _need_ the real hardware to run them, should use a Hardware In Loop testing setup instead.

## Hardware-in-Loop Testing

Hardware In Loop (HIL) testing is the use of real devices in the testing setup. We use the [`embedded-test`] framework to write unit and integration tests, so in principle, the process is only slightly different from normal non-embedded projects. We use [`probe-rs`] (check correct revision in [`hil-test` sub-repository]) to flash and run tests on the target device. To do this, you **must** use **only** the `USB-Serial-JTAG` port on your DevKit (see [Hardware Overview](../introduction/hardware-overview.md)). If your device does not have such a port, you will have to use [esp-prog] or another suitable programmer and connect it according to the [connection instructions] (select the desired chip on the page). 

Using `esp-generate`, and selecting `embedded-test` under `probe-rs` will set up testing in your project for you, such that you can just run `cargo test` locally when the device is connected correctly.

### `embedded-test`

[`embedded-test`] tests are written as functions placed under the `#[test]` macro, similar to the `std` test framework. Typically, the result of the test is the result of this function, i.e. if the code panicked, the test is considered failed and the next test is run. However, there are many ways to customize this process, such as the `#[should_panic]` attribute macro, where `panic` is considered a successful test completion, setting timeouts for tests, and much more, which can be found in the [`embedded-test` documentation]. It also supports integration with IDE, as it mimics the default Rust test harness.

[`embedded-test`]: https://github.com/probe-rs/embedded-test
[`probe-rs`]: https://probe.rs
[`hil-test` sub-repository]: https://github.com/esp-rs/esp-hal/tree/main/hil-test
[esp-prog]:  https://docs.espressif.com/projects/esp-dev-kits/en/latest/other/esp-prog/user_guide.html
[connection instructions]: https://docs.espressif.com/projects/esp-idf/en/v5.2.3/esp32s2/api-guides/jtag-debugging/configure-other-jtag.html
[`embedded-test` documentation]: https://docs.rs/embedded-test/0.6.2/embedded_test/