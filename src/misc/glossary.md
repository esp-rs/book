# Appendix A: Glossary

A number of acronyms are used in the embedded development space. This glossary attempts to define any acronyms used in this book.

## SVD

**S**ystem **V**iew **D**escription.

The [CMSIS-SVD] specification formalizes the description of the system contained within a microcontroller. This specification was designed with ARM Cortex-M microcontrollers in mind, however, it is still applicable to other architectures.

SVD files are XML and contain definitions for peripherals which can be consumed by tools such as [svd2rust] to generate Peripheral Access Crates.

[CMSIS-SVD]: https://arm-software.github.io/CMSIS_5/SVD/html/index.html
[svd2rust]: https://github.com/rust-embedded/svd2rust/

## PAC

**P**eripheral **A**ccess **C**rate.

Provides a type-safe, low-level API for interacting with the device's hardware peripherals. For more information on the generated API please refer to the [svd2rust] documentation.

## HAL

**H**ardware **A**bstraction **L**ayer.

Provides higher-level abstractions over hardware peripherals which are more easily used by developers. These libraries are generally implemented on top of Peripheral Access Crates, and often implement the various traits provided by [embedded-hal].

[embedded-hal]: https://github.com/rust-embedded/embedded-hal
