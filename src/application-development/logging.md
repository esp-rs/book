# Logging

​Logging is a crucial aspect of embedded systems development, providing visibility into the system's behavior and aiding in debugging and monitoring. In the Rust ecosystem, two prominent logging frameworks are commonly used: [`defmt`][defmt] and [`log`][log].

Regardless of which of the following tools you choose to use when generating your project, [`esp-generate`][esp-generate] will make sure that everything is set up correctly, so that the end user only has to adapt the logging tool used to their needs.

## `defmt`

[`defmt`][defmt] is a highly efficient logging framework designed for resource-constrained environments, such as embedded systems. It offers compact, binary-encoded log messages, reducing the overhead associated with traditional string-based logging.​ For more information about see [`defmt` documentation][defmt-documentation]. We recommend pairing `defmt` with `probe-rs` for the best results on Espressif chips.

## `log`

The [`log`][log] crate is a widely adopted logging facade in the Rust community. It defines a set of macros (`info!`, `warn!`, `error!`, etc.) to capture log messages at various levels. We offer a [logger] implementation in `esp-println`, but you are free to implement your own logger if you wish.

[log]: https://crates.io/crates/log
[defmt]: https://crates.io/crates/defmt
[defmt-documentation]: https://defmt.ferrous-systems.com/introduction
[filtering]: https://defmt.ferrous-systems.com/filtering#filtering
[esp-generate]: ./../getting-started/tooling/esp-generate.md
[logger]: https://docs.rs/log/latest/log/#implementing-a-logger
