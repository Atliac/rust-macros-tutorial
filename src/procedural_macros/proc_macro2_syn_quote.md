# The Procedural Macro Ecosystem: proc-macro2, syn, and quote

Procedural macros function by manipulating the `proc_macro::TokenStream` type. However, the standard `proc_macro` crate is a compiler built-in and is restricted to crates explicitly defined as procedural macro libraries. To bypass these limitations and streamline development, the Rust ecosystem relies on three foundational crates:

- **`proc-macro2`**: This crate offers an API almost identical to `proc_macro` but can be used in any environment (including tests and binaries). It acts as a bridge, facilitating seamless conversion to and from the compiler-native `proc_macro::TokenStream`.
- **`syn`**: A parsing library that provides a complete syntax tree for any valid Rust source code. It also makes defining custom syntax trees easy.
- **`quote`**: As the counterpart to `syn`, this crate enables you to convert Rust code templates back into a `proc_macro2::TokenStream`. Its support for "quasi-quoting" allows you to interpolate variables directly into the generated source code.
