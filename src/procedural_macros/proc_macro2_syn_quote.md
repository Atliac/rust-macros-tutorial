# proc-macro2, syn, and quote

Procedural macros handle the `TokenStream` from the `proc-macro` crate. This is a compiler-provided crate that can only be used within a procedural macro crate.

The `proc-macro2` crate, which can be used in any type of crate, provides a similar API to `proc-macro::TokenStream`. It allows for easy conversion to and from `proc-macro::TokenStream`.

`syn` is used to parse a `proc-macro` or `proc-macro2` `TokenStream` into an Abstract Syntax Tree (AST), making it much easier to process.

`quote` converts Rust code back into a `proc-macro2::TokenStream`, allowing you to interpolate variables from your AST directly into the code.
