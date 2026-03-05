# Frequently Asked Questions

This chapter addresses common questions and challenges that developers may encounter when working with Rust on ESP chips. Whether you're setting up your development environment, optimizing your code, or looking to simulate your projects, you'll find practical solutions and best practices here.

## Editor/IDE

When using [`esp-generate`][esp-generate], you can automatically configure recommended settings and extensions for VS Code, Helix, Neovim, and Zed editors during the generation process.

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

The `mem::forget` function should be avoided, as forgetting drivers may result in unintended consequences. Peripheral drivers provide `Drop` implementations which return the peripheral to its default, unconfigured state, and if necessary cancel any Direct Memory Access (DMA) transactions which are current in progress. Forgetting a driver may result in erroneously configured peripherals and/or DMA transactions which run indefinitely and never complete.

## Moving Code and Data to RAM

By default, static data and the majority of application code reside in Flash memory. While this
saves space, accessing Flash can incur a performance penalty due to cache misses. Moving
critical paths to RAM improves execution speed but consumes a very limited hardware resource.
Finding the right balance is application-specific.

Implementation Methods

1. For your own code: The `#[ram]` Macro

   For functions or data within your own crate, the simplest method is to use the `#[ram]`
   attribute macro. This automatically handles the placement for you.

2. For external code: Linker Hooks

   If you need to move code or data that you don't control (e.g., from a third-party
   dependency), you can hook into the `esp-hal` linker scripts.

   - Enable the corresponding config options (`ESP_HAL_CONFIG_USE_RWDATA_LD_HOOK` / `ESP_HAL_CONFIG_USE_RWTEXT_LD_HOOK`)
     to require the linker to look for `rwdata_hook.x` (for data) or `rwtext_hook.x` (for code).
   - These files must use the [Linker Input Section syntax](https://sourceware.org/binutils/docs/ld/Input-Section-Basics.html).

   Content in these files is included directly into the `.data` and `.rwtext` sections without
   validation.

   Example `rwtext_hook.x` content:
   ```text
   /* Move all functions from the 'some_critical_lib' to RAM */
   *:some_critical_lib.*(.literal .literal.* .text .text.*)
   ```

   Configuration & Pathing
   By default, the linker looks for these hook files in your project’s current working
   directory. If you prefer to store them elsewhere, add the directory to your linker search
   path in `.cargo/config.toml`:

   ```toml
   rustflags = [
       "-C", "link-args=-L./hooks",
   ]
   ```

   It's advised to perform a clean build after modifying the hook files to guarantee the
   changes are picked up correctly.

   To check the effects of the changes it's useful to generate a linker map file.

   Xtensa:
   ```toml
   rustflags = [
       "-C", "link-arg=-Wl,-Map=output.map",
   ]
   ```

   RISC-V:
   ```toml
   rustflags = [
       "-C", "link-arg=-Map=output.map",
   ]
   ```

   <section class="warning">
   These hooks provide direct access to the linking process. Incorrect syntax or
   over-allocating RAM can lead to link-time errors or application instability.
   </section>
