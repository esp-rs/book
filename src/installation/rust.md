# Rust Installation

Make sure you have [Rust][rust-lang-org] installed. If not, see the instructions on the [rustup][rustup.rs-website] website.

> 🚨 **Warning**: When using Unix based systems, installing Rust via a system package manager (e.g. `brew`, `apt`, `dnf`, etc.) can result in various issues and incompatibilities, so it's best to use [rustup][rustup.rs-website] instead.

When using Windows, make sure you have installed one of the ABIs listed below. For more details, see the [Windows][rustup-book-windows] chapter in The rustup book.
- **MSVC**: Recommended ABI, included in the list of `rustup` default requirements. Use it for interoperability with the software produced by Visual Studio.
- **GNU**: ABI used by the GCC toolchain. Install it yourself for interoperability with the software built with the MinGW/MSYS2 toolchain.

See also [alternative installation methods][rust-alt-installation].

[rustup.rs-website]: https://rustup.rs/
[rust-alt-installation]: https://rust-lang.github.io/rustup/installation/other.html
[rustup-book-windows]: https://rust-lang.github.io/rustup/installation/windows.html
[rust-lang-org]: https://www.rust-lang.org/
