# Hello World

In the last chapter you flashed and run your first piece of code on the SoC - while that is already really exciting we can do better.

Traditionally the first thing to run on a microcontroller is _blinky_.

However we will start with _Hello World_ here.

## Add a Dependency

In `Cargo.toml` in the `[dependencies]` section add this line:

```toml
esp-println = { version = "0.3.1", features = ["esp32c3"] }
```

`esp-println` is an additional crate that calls ROM functions to print text that is shown by `espflash` or `espmonitor` (or any other serial monitor).

We need to pass the feature `esp32c3` since that crate targets multiple SoCs and needs to know which one it is supposed to run on.

## Print Something

In `main.rs` before the `loop {}` add this line
```rust,ignore
esp_println::println!("Hello World");
```

Please note: Both `espflash` and `espmonitor` both only show output when a new-line is detected. There is also a `print!` macro but nothing will be shown until a new-line is received.

However other serial monitors work differently.

## See Results

Again run
```shell
cargo run
```

You should see the text _Hello World_ printed!
