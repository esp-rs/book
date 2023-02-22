# Panic!

When something goes terribly wrong in Rust there might occur a [panic].

Let's see what it looks like for us.

In `main.rs` put this line somewhere, e.g. before our `println`

```rust,ignore
panic!("This is a panic");
```

Again run the code.

You should see something like this

```text


!! A panic occured in 'src\main.rs', at line 25, column 5

PanicInfo {
    payload: Any { .. },
    message: Some(
        This is a panic,
    ),
    location: Location {
        file: "src\\main.rs",
        line: 25,
        col: 5,
    },
    can_unwind: true,
}

Backtrace:

0x420019aa
0x420019aa - main
    at C:\tmp\getting-started\src\main.rs:25
0x4200014c
0x4200014c - _start_rust
    at ...\.cargo\registry\src\github.com-1ecc6299db9ec823\riscv-rt-0.9.0\src\lib.rs:389
```

We see where the panic occured and we even see a backtrace!

While in this example things are obvious, this will come handy in more complex code.

Now try running the code compiled with release profile.
```shell
cargo run --release
```

Now things are less pretty:
```text

!! A panic occured in 'src\main.rs', at line 25, column 5

PanicInfo {
    payload: Any { .. },
    message: Some(
        This is a panic,
    ),
    location: Location {
        file: "src\\main.rs",
        line: 25,
        col: 5,
    },
    can_unwind: true,
}

Backtrace:

0x42000140
0x42000140 - _start_rust
    at ??:??
```

We still see where the panic occured but the backtrace is less helpful now.

That is because the compiler omitted debug information and optimized the code.

But you might have noticed the difference in the size of the flashed binary.

It went from 199056 bytes down to 86896 bytes!

Please note that this is still huge for what we get. There are a lot of options to get the binary smaller which is beyond the scope of this book.
<!-- (TODO: should we add a section about binary sizes?) -->

Before going further remove the line causing the explicit panic.

[panic]: https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html
