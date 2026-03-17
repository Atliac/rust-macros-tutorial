# The Procedural Macro Ecosystem: proc-macro2, syn, and quote

Procedural macros function by manipulating the `proc_macro::TokenStream` type. However, the standard `proc_macro` crate is a compiler built-in and is restricted to crates explicitly defined as procedural macro libraries. To bypass these limitations and streamline development, the Rust ecosystem relies on three foundational crates:

- **`proc-macro2`**: This crate offers an API almost identical to `proc_macro` but can be used in any environment (including tests and binaries). It acts as a bridge, facilitating seamless conversion to and from the compiler-native `proc_macro::TokenStream`.
- **`syn`**: A robust parsing library that transforms a `TokenStream` into a complete syntax tree. This allows you to process and manipulate Rust code programmatically, which is significantly more intuitive than working with raw tokens.
- **`quote`**: As the counterpart to `syn`, this crate enables you to convert Rust code templates back into a `proc_macro2::TokenStream`. Its support for "quasi-quoting" allows you to interpolate variables directly into the generated source code.

## A First Look at Syntax Trees

```rust,editable
use quote::quote;

fn main() {
    let token_stream = quote! {
        fn main(){
            println!("Hello, world!");
        }
    };

    let syntax_tree: syn::File = syn::parse2(token_stream).unwrap();

    println!("{:#?}", syntax_tree);
}
```

> [!TIP]
> While the output may seem overwhelming at first, remember that this is only a demonstration. In practice, we rarely need to work with syntax trees at the `syn::File` level.
