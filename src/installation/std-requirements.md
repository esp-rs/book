# `std` Development Requirements

Regardless of the target architecture, make sure you have the following required tools installed to build [`std`][rust-esp-book-overview-std] applications:

- ESP-IDF Prerequisites:
  - Windows: [`python`][python-website-download] and [`git`][git-website-download]
  - Linux: See [Linux ESP-IDF prerequisites][esp-idf-linux].
  - macOS: See [macOS ESP-IDF prerequisites][esp-idf-macos].
- [`ldproxy`][embuild-github-ldproxy] binary crate: A tool that forwards linker arguments to the actual linker that is also given as an argument to `ldproxy`. Install it by running:
    ```shell
    cargo install ldproxy
    ```

> ⚠️ **Note**: The `std` runtime uses [ESP-IDF][esp-idf-github] (Espressif IoT Development Framework) as hosted environment but, users don't need to install it. ESP-IDF is automatically downloaded and installed by [`esp-idf-sys`][esp-idf-sys-github], a crate that all `std` projects need to use, when building a `std` application.

[rust-esp-book-overview-std]: ../overview/using-the-standard-library.md
[python-website-download]: https://www.python.org/downloads/windows/
[git-website-download]: https://git-scm.com/downloads
[embuild-github-ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys-github]: https://github.com/esp-rs/esp-idf-sys
[esp-idf-github]: https://github.com/espressif/esp-idf
[esp-idf-linux]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/linux-macos-setup.html#for-linux-users
[esp-idf-macos]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/linux-macos-setup.html#for-macos-users

