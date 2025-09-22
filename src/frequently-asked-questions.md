# Frequently Asked Questions

This chapter addresses common questions and challenges that developers may encounter when working with Rust on ESP chips. Whether you're setting up your development environment, optimizing your code, or looking to simulate your projects, you'll find practical solutions and best practices here.

## Editor/IDE

When using [`esp-generate`][esp-generate], you can automatically configure recommended settings and extensions for VS Code, Helix, and Zed editors during the generation process.

[esp-generate]: ./getting-started/tooling/esp-generate.md

## Size and Memory Optimizations

### Optimizing Binary Size

- Cargo provides some default profiles; we recommend using the [`release` profile][release-profile] as it optimizes and removes debug symbols.
- Cargo allows different [profile settings][profile-settings-cargo], which can make a difference in the resulting size of the artifact.
  - See [Optimizations: the speed size tradeoff][embedded-book-tradeoffs] of The Embedded Rust Book.
- Be careful when using external dependencies, as they can increase the size of your resulting artifact.
- Filter log messages if they are not going to be useful or read.
- More suggestions can be found in the [min-sized-rust][min-sized-rust] repository.

Additionally, the [Embassy documentation][embassy-documentation] contains some suggestions with regards to binary sizes in the [Frequently Asked Questions][frequently-asked-questions] section.

[embedded-book-tradeoffs]: https://docs.rust-embedded.org/book/unsorted/speed-vs-size.html
[release-profile]: https://doc.rust-lang.org/cargo/reference/profiles.html#release
[profile-settings-cargo]: https://doc.rust-lang.org/cargo/reference/profiles.html#profile-settings
[min-sized-rust]: https://github.com/johnthagen/min-sized-rust
[embassy-documentation]: https://embassy.dev/book
[frequently-asked-questions]: https://embassy.dev/book/#_frequently_asked_questions

### Optimizing Memory Usage

We will, again, defer to the [Embassy Documentation][embassy-documentation], specifically the [How can I measure resource usage (CPU, RAM, etc.)?][measure-resources] section.

[measure-resources]: https://embassy.dev/book/#_how_can_i_measure_resource_usage_cpu_ram_etc

## Using Crates from Git

The [Cargo Book][cargo-book] and the [Embassy Documentation][embassy-documentation] both contain information on how to specify dependencies from Git repositories:

- [Specifying dependencies from `git` repositories][dependencies-from-git]
- [The `[patch]` section][patch-section]
- [How do I switch to the `main` branch][switch-to-main-branch]

[cargo-book]: https://doc.rust-lang.org/cargo/
[dependencies-from-git]: https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#specifying-dependencies-from-git-repositories
[patch-section]: https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html#the-patch-section
[switch-to-main-branch]: https://embassy.dev/book/#_how_do_i_switch_to_the_main_branch

## Can I Use `mem::forget` on Drivers?

The `mem::forget` function should be avoided, as forgetting drivers may result in unintended consequences. Peripheral drivers provide `Drop` implementations which return the peripheral to its default, unconfigured state, and if necessary cancel any DMA transactions which are current in progress. Forgetting a driver may result in erroneously configured peripherals and/or DMA transactions which run indefinitely and never complete.
