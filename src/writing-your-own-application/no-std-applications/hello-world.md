# Hello World

In the last chapter you flashed and run your first piece of code on the SoC - while that is already really exciting we can do better.

Traditionally the first thing to run on a microcontroller is _blinky_.

However, we will start with _Hello World_ here.

## Add a Dependency
You can add a dependency by any of the following methods:
- Editing `Cargo.toml`
In `Cargo.toml` in the `[dependencies]` section add this line:
```toml
esp-println = { version = "0.3.1", features = ["esp32c3"] }
```
- Using [`cargo add`]
```sh
cargo add esp-println --features "esp32c3"
```

[`esp-println`] is an additional crate that calls ROM functions to print text that is shown by [`espflash`] (or any other serial monitor).

We need to pass the feature `esp32c3` since that crate targets multiple SoCs and needs to know which one it is supposed to run on.

> Note that there might be new versions by the time you are reading this, please check [crates.io].

## Print Something

In `main.rs` before the `loop {}` add this line
```rust,ignore
esp_println::println!("Hello World");
```

> Note: [`espflash`] only shows output when a new-line is detected. There is also a `print!` macro but nothing will be shown until a new-line is received.

However other serial monitors work differently.

## See Results

Again run
```shell
cargo run
```

You should see the text _Hello World_ printed!

[`espflash`]: https://github.com/esp-rs/espflash
[`esp-println`]: https://github.com/esp-rs/esp-println
[crates.io]: https://crates.io/crates/esp-println
[`cargo add`]: https://doc.rust-lang.org/cargo/commands/cargo-add.html
