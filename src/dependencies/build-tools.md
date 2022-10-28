# Build Tools

## ldproxy

When building applications using the Rust standard library, `std`, the build tool `ldproxy` is required and must be installed first; this tool can be found in the [embuild repository]. This tool is _not_ required for `no_std` applications.

`ldproxy` is a simple tool to forward linker arguments given to `ldproxy` to the actual linker executable.

To install:

```bash
cargo install ldproxy
```

[embuild repository]: https://github.com/esp-rs/embuild
