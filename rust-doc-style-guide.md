# Rust Documentation Style Guide

As [The Rust RFC Book](https://rust-lang.github.io/rfcs/2436-style-guide.html#drawbacks) states:

> One can level some criticisms at having a style guide:
>
> - It is bureaucratic, gives developers more to worry about, and crushes creativity.
> - There are edge cases where the style rules make code look worse (e.g., around FFI).
>
> However, these are heavily out-weighed by the benefits.

The style guide is based on the best practices collected from the following books:

- [The Rust Programming Language](https://doc.rust-lang.org/book/foreword.html)
- [The Embedded Rust Book](https://docs.rust-embedded.org/book/intro/index.html)
- [The rustup book](https://rust-lang.github.io/rustup/installation/windows.html)
- [The Cargo Book](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html)
- [The rustc book](https://doc.rust-lang.org/nightly/rustc/targets/index.html)
- [The Rust on ESP Book](https://esp-rs.github.io/book/)

## Contents of This Style Guide

- [Rust Documentation Style Guide](#rust-documentation-style-guide)
  - [Contents of This Style Guide](#contents-of-this-style-guide)
  - [Heading Titles](#heading-titles)
    - [Capitalization](#capitalization)
  - [Linking](#linking)
    - [Adding Links](#adding-links)
    - [Formatting](#formatting)
  - [Lists](#lists)
    - [Types](#types)
    - [Formatting](#formatting-1)
  - [Using `monospace`](#using-monospace)
    - [Monospace and Other Types of Formatting](#monospace-and-other-types-of-formatting)
  - [Using _Italics_](#using-italics)
  - [Mode of Narration](#mode-of-narration)
  - [Terminology](#terminology)
    - [Recommended Terms](#recommended-terms)
  - [Appenndix A Existing Style Guides](#appenndix-a-existing-style-guides)
    - [Documentation](#documentation)
    - [Code](#code)


## Heading Titles

The books on Rust usually have the heading titles based on nouns or gerunds:

> #### Design Patterns
> #### Using Structs to Structure Related Data

### Capitalization

In heading titles, capitalize the first letter of every word **except for**:

- Articles (a, an, the); unless an article is the first word.

  > #### Defining an Enum
  > #### A First Attempt at Rust

- Coordinating conjunctions (and, but, for, or, nor).

  > #### Packages and Crates

- Prepositions of four letters or less; unless these prepositions are the first or last words.
  - Prepositions of _five_ letters and above should be capitalized (Before, Through, Versus, Among, Under, Between, Without, etc.).

  > #### Peripherals as State Machines

Do not capitalize names of functions, commands, packages, websites, etc.

> #### What is `rustc`
> #### Publishing on crates.io

See also, the [Monospace and Other Formatting](#monospace-and-other-formatting) section.

In hyphenated words, do not capitalize the parts following the hyphens.

> #### Built-in Targets
> #### Allowed-by-default Lints


## Linking

### Adding Links

The books on Rust usually use [link variables][stackoverflow-link-var] defined right after the paragraph where they are used. For variables, use names that give a clue on where the link leads. It will simplify link maintenance.

[stackoverflow-link-var]: https://stackoverflow.com/a/27784490/10308406

> [`espup`][espup-github] is a tool that simplifies installing and maintaining the components required to develop Rust applications.

> [espup-github]: https://github.com/esp-rs/espup

### Formatting

The books on Rust usually use the following link formatting:

> As mentioned in the [Environment Variables](https://rust-lang.github.io/rustup/installation/index.html) section, ...

> For more details, see the [Windows](rustup-book-windows) chapter in The rustup book.

[book-ja](https://github.com/rust-lang-ja/book-ja/blob/master-ja/style-guide.md) also suggests:

- Make intra-book links relative, so they work both online and locally

Do NOT turn long phrases into links

  > ❌ See the [Rust Reference’s section on constant evaluation](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html) for more information on what operations can be used when declaring constants.

Also take into account the following

- Do not provide a link to the same location repeatedly in the same or adjacent paragraphs without a good reason, especially using different link text.
- Do not use the same link text to refer to different locations.

  > `espup` might have a section in a book and a github repo. In this case, see the [`espup`](#espup) section and [`espup` repo](https://hackmd.io/-cf0jVTqRqm9O7GM5JgLiA?both).

See also, the [Monospace and Other Formatting](#monospace-and-other-formatting) section.

## Lists

### Types

The following types of lists are usually used in documentation:

- **Bullet list** -- use it if the order or items is not important
- **Numbered list** -- use it if the order of items is important, such as when describing a process
  - **Procedure** -- special type of numbered list that gives steps to achieve some goal (to achieve this, do this); for an example of a procedure, see the [Usage](https://doc.rust-lang.org/nightly/rustc/profile-guided-optimization.html#usage) section in The rustc book.

### Formatting

The books on Rust usually use the following list formatting:

- Finish an introductory sentence with a colon.
- Capitalize the first letter of each bullet point.

  > Using C or C++ inside of a Rust project consists of two major parts:

  > - Wrapping the exposed C API for use with Rust
  > - Building your C or C++ code to be integrated with the Rust code

- If a bullet point is a full sentence, you can end it with a full stop.
  - If a list has at least one full stop, end all other list items with a full stop.

  > To reliably interact with these peripherals:

  > - Always use `volatile` methods to read or write to peripheral memory, as it can change at any time.
  > - In software, we should be able to share any number of read-only accesses to these peripherals.
  > - If some software should have read-write access to a peripheral, it should hold the only reference to that peripheral.

- For longer list items, consider using a summary word of phrase to make content [scannable](https://learn.microsoft.com/en-us/style-guide/scannable-content/).

  > If you run Windows on your host machine, make sure ...
  >
  > - **MSVC**: Recommended ABI, included in ...
  > - **GNU**: ABI used by the GCC toolchain ...

  -  For an example using bold font, see the list in the [Modules Cheat Sheet](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html#modules-cheat-sheet) section in The Rust Programming Language book.
  -  For an example using monospace font, see the [Panicking](https://docs.rust-embedded.org/book/start/panicking.html#panicking) section in The Embedded Rust Book.


## Using `monospace`

Use monospace font for the following items:

- Code snippets

  The [Installation](https://doc.rust-lang.org/book/ch01-01-installation.html) chapter in the Rust Programming Language book suggests:

  - Start the terminal commands with `$`
  - Output of previous commands should not start with `$`
  - For PowerShell-specific examples, use `>` instead of `$`

  [book-ja](https://github.com/rust-lang-ja/book-ja/blob/master-ja/style-guide.md) also suggests:

  - Use `bash` syntax highlighting

- Rust declarations: commands, functions, arguments, parameters, flags, variables
- In-line command line output

  > Writing a program that prints `Hello, world!`

- Data types: `i8`, `u128`
- Names of crates, traits, libraries,
- Command line tools, plugins, packages
- Files: `config.toml`
- Architectures and targets

 > The `RISC-V` and `Xtensa` architectures.

### Monospace and Other Types of Formatting

Monospace font can also be used in:

- links

  > [`String`](https://doc.rust-lang.org/std/string/struct.String.html) is a string type provided by ...

- headings

  > ### A `no_std` Rust Environment


## Using _Italics_

- Introduce new terms

  > Each value in Rust has an _owner_.
  > A _hash map_ allows you to associate a value with a particular key.

- Emphasize important concepts or words

  > When `s` comes _into_ scope, it is valid. It remains valid until it goes _out of_ scope.


## Mode of Narration

- Use _the first person_ (we) when introducing a tutorial or explaining how things will be done. The reader will feel like being on the same team with the authors working side by side.

  > We'll start writing a program for the LM3S6965, a Cortex-M3 microcontroller. We have chosen this as our initial target because it can be emulated using QEMU so you don't need to fiddle with hardware in this section, and we can focus on the tooling and the development process.

- Use _the second person_ (you) when describing what the reader should do while installing software, following a tutorial or a procedure. However, in most cases you can use imperative mood as if giving orders to the readers. It makes instructions much shorter and clearer.

  > 1\. Install `cargo-generate`
  >
  > `cargo install cargo-generate`
  >
  > 2\. Generate a new project
  >
  > `cargo generate --git https://github.com/rust-embedded/cortex-m-quickstart`

- Use _the third person_ (the user, it) when describing how things work from the perspective of hardware or software

  > A driver has to be initialized with an instance of type that implements a certain `trait` of the embedded-hal which is ensured via trait bound and provides its own type instance with a custom set of methods allowing to interact with the driven device.


## Terminology

This chapter lists the terms that have inconsistencies in spelling, usage, etc.

If you spot other issues with terminology, please add the terms here in alphabetical order using the formatting as follows:

- _Recommended term_
  - Avoid: Add typical phrases in which this term is found
  - Use: Add recommended phrases
  - Note: Add more information if needed

### Recommended Terms

- Cargo
  - Avoid: cargo
- ESP-IDF or esp-idf
  - Use: esp-idf when writing about the esp-idf repo
  - Use: ESP-IDF when writing about the ESP-IDF framework or ESP-IDF programming guide
- _Product_
  - Avoid: Espressif SoCs
  - Use: Espressif products
  - Note: In the content of running applications, suggested as an umbrella term for Espressif chips, modules, development boards, etc. Otherwise, it might be potentially ambiguous that running an app on an SoC is the same as running it on a module.
- _SoC_
  - Avoid: _chip_
  - Note: see also _product_


## Appenndix A Existing Style Guides

### Documentation

- [book-ja](https://github.com/rust-lang-ja/book-ja/blob/master-ja/style-guide.md)

### Code

- [Style Guidelines](https://doc.rust-lang.org/1.0.0/style/README.html)
- [The Rust RFC Book](https://rust-lang.github.io/rfcs/2436-style-guide.html), chapter _Style Guide_
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- [Rust Style Guide](https://riptutorial.com/rust/topic/4620/rust-style-guide) (riptutorial.com)
- [Rust Style Guide](https://github.com/rust-lang/style-team/blob/master/guide/guide.md) (github.com/rust-lang)

