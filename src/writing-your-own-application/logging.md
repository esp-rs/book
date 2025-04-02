# Logging

​Logging is a crucial aspect of embedded systems development, providing visibility into the system's behavior and aiding in debugging and monitoring. In the Rust ecosystem for ESP devices, two prominent logging frameworks are commonly used: the [log] crate and [defmt]. The [esp-println] crate serves as a bridge, facilitating logging through these frameworks on ESP devices.​

## The `log` crate

The [log] crate is a widely adopted logging facade in the Rust community. It defines a set of macros (`info!`, `warn!`, `error!`, etc.) to capture log messages at various levels. The actual logging behavior is determined by the selected backend implementing the `Log` trait.

### Integrating `log` with `esp-println`

To utilize the `log` crate with `esp-println`, follow these steps:​

1. Add Dependencies to your `Cargo.toml`:
```toml
esp-hal          = { version = "1.0.0-beta.0", features = ["esp32c6"] }
esp-println      = { version = "0.13.0", features = ["esp32c6", "log"] }
log              = { version = "0.4.21" }
```
Ensure you specify the feature corresponding to your target device (e.g. `esp32c6` for ESP32-C6).

2. Set log level in your `.config.toml`:
```toml
[env]
ESP_LOG="info" # Set desired log level
```

3. Initialize the Logger:
```rust
    esp_println::logger::init_logger_from_env();
```

4. Minimal example:
```rust
use log::info;

#[main]
fn main() -> ! {
    
    esp_println::logger::init_logger_from_env();
    info!("Hello world!");
    // ...
}
```

## The `defmt` Framework

[defmt] is a highly efficient logging framework designed for resource-constrained environments, such as embedded systems. It offers compact, binary-encoded log messages, reducing the overhead associated with traditional string-based logging.​ For more information about see [defmt-documentation].

### Integrating `defmt` with `esp-println`

`esp-println` can be configured to work seamlessly with `defmt`, providing a global logger that outputs defmt-encoded messages. To set this up:​

1. Add dependencies to your `Cargo.toml`:
```toml
defmt            = "0.3.10"
esp-hal          = { version = "1.0.0-beta.0", features = ["defmt", "esp32c6"] }
esp-println      = { version = "0.13.0", features = ["defmt-espflash", "esp32c6"] }
```
Ensure you specify the feature corresponding to your target device (e.g. `esp32c6` for ESP32-C6).

2. Set log level in your `.config.toml`:
```toml
[env]
DEFMT_LOG="info" # Set desired log level
```
For more details about filtering see [filtering].

3. Minimal example:
```rust
use defmt::info;
use esp_println as _;

#[main]
fn main() -> ! {
    
    info!("Hello world!");
    // ...
}
```

### Integrating `defmt` with `probe-rs`

1. Add dependencies to your `Cargo.toml`:
```toml
defmt            = "0.3.10"
esp-hal          = { version = "1.0.0-beta.0", features = ["defmt", "esp32c6"] }
rtt-target       = { version = "0.6.1", features = ["defmt"] }
```

2. Set log level in your `.config.toml`:
```toml
[env]
DEFMT_LOG="info" # Set desired log level
```
For more details about filtering see [filtering].

3. Initialize the Logger
```rust
    rtt_target::rtt_init_defmt!();
```

4. Minimal example:
```rust
use defmt::info;
#[main]
fn main() -> ! {
    
    rtt_target::rtt_init_defmt!();
    info!("Hello world!");
    // ...
}
```

[log]: https://crates.io/crates/log
[defmt]: https://crates.io/crates/defmt
[esp-println]: https://crates.io/crates/esp-println
[defmt-documentation]: https://defmt.ferrous-systems.com/introduction
[filtering]: https://defmt.ferrous-systems.com/filtering#filtering