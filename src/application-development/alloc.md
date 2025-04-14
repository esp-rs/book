# Alloc

In a `no_std` environment, the [`alloc`][alloc] crate is available as an option for heap allocation. It can be useful when working with crates that require alloc or when using dynamic collections like `Vec`.

We provide our own `no_std` heap allocator, [`esp-alloc`][esp-alloc]. To use it, you need to:

1. Add a dependency to your `Cargo.toml`
```toml
esp-alloc        = "0.7.0"
```

2. Modify the `.cargo/config.toml`
```toml
[unstable]
build-std = ["alloc", "core"] # added alloc here
```

3. Use `alloc` crate in your application
```rust
// main.rs

extern crate alloc;
```

4. Initialize a global heap allocator providing a heap of the given size in bytes with the provided macro:
```rust
// main.rs

esp_alloc::heap_allocator!(size: 72 * 1024);
```

or when more customization is required using the function:
```rust
// main.rs

use esp_alloc as _;
fn init_heap() {
    const HEAP_SIZE: usize = 32 * 1024;
    static mut HEAP: MaybeUninit<[u8; HEAP_SIZE]> = MaybeUninit::uninit();
    unsafe {
        esp_alloc::HEAP.add_region(esp_alloc::HeapRegion::new(
            HEAP.as_mut_ptr() as *mut u8,
            HEAP_SIZE,
            esp_alloc::MemoryCapability::Internal.into(),
        ));
    }
}
```

## Configurable Memory Placement and Reclaimed RAM

On chips with non-contiguous memory, `cfg` options can be used to control where memory is placed. This also allows accessing RAM that is otherwise unavailable, such as memory occupied by the 2nd stage bootloader. For supported chips, regions like `.dram2_uninit` can be used as additional heap memory, optimizing available resources.

```rust
// Use 64kB in dram2_seg for the heap, which is otherwise unused.
heap_allocator!(#[link_section = ".dram2_uninit"] size: 64000);
```

## PSRAM

Our chips have a few hundred kilobytes of internal RAM, which could be insufficient for some applications. The ESP32, ESP32-S2, and ESP32-S3 have the ability to use virtual addresses for external PSRAM (Psuedostatic RAM) memory. The external memory is usable is the same way as internal data RAM, with certain restrictions. ESP32 restrictions can be found [here].

### Allocator Considerations

You can only have **one global allocator** but the allocator can use multiple regions (e.g. PSRAM, internal RAM or even multiple blocks of them). You can use multiple allocators with the nightly-feature "allocator api" and with [`allocator api2`][allocator api2], which [`esp-alloc`][esp-alloc] implements.

[esp-alloc]: https://crates.io/crates/esp-alloc
[alloc]: https://doc.rust-lang.org/alloc/
[here]: https://docs.espressif.com/projects/esp-idf/en/v5.4.1/esp32/api-guides/external-ram.html#restrictions
[allocator api2]: https://crates.io/crates/allocator-api2