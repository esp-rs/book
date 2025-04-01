# Alloc

In `no_std`, we need to rely on `alloc` crate, which provides heap allocation support. To use `alloc` in `no_std` environment:

1. Explicitly enable the alloc crate
2. Provide a Global Allocator

We provide our own simple `no_std` heap allocator [esp-alloc]. To use it you need to:

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

4. Initialize a global heap allocator providing a heap of the given size in bytes (with macro, preferrable)
```rust
// main.rs

esp_alloc::heap_allocator!(size: 72 * 1024);
```

or (when more flexibility is needed)
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

[esp-alloc]: https://crates.io/crates/esp-alloc