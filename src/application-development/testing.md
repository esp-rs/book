# Testing

Testing is an integral part of application development. Using the example of how we do it at esp-hal, user can test their applications too.

## HIL testing

Testing in our ecosystem is organised using Hardware-in-loop (HIL). We use the [`embedded-test`] framework to write unit and integration tests, so in principle, the process is only slightly different from normal non-embedded projects.

We use [`probe-rs`] (revision `#9bde591`) to flash and run tests on the target device. To do this, the user **MUST** use **only** the `USB-Serial-JTAG` port on their devkit (usually the right one, possibly labelled "USB" next to it). If user's device does not have such a port, they will have to use [esp-prog] and connect it according to the [connection instructions] (select the desired chip on the page).

The test itself is written as a function placed under the `#[test]` macro. Typically, the result of the test is the result of this function, i.e. if the code panicked, the test is considered failed and the next test is run. However, there are many ways to customize this process, such as the `#[should_panic]` attribute macro, where `panic` is considered a successful test completion, setting timeouts for tests, and much more, which can be found in the [`embedded-test` documentation]. It also supports integration with IDE, as it mimics the default Rust test harness. You can learn more about this in the [project repository][`embedded-test`].

[`embedded-test`]: https://github.com/probe-rs/embedded-test
[`probe-rs`]: https://probe.rs
[esp-prog]:  https://docs.espressif.com/projects/esp-dev-kits/en/latest/other/esp-prog/user_guide.html
[connection instructions]: https://docs.espressif.com/projects/esp-idf/en/v5.2.3/esp32s2/api-guides/jtag-debugging/configure-other-jtag.html
[`embedded-test` documentation]: https://docs.rs/embedded-test/0.6.2/embedded_test/