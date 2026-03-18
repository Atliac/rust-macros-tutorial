# TokenStream

**TLDR**: A procedural macro is a function that takes a (or two for attribute macros) `TokenStream` as input and returns a `TokenStream` as output.

Before the compiler calls our procedural macro, it converts the source code (the code to which the macro is applied) into a `TokenStream`. The compiler then calls our macro with this `TokenStream` as an argument. Finally, our procedural macro returns a new `TokenStream` as its result.

A `TokenStream` is roughly equivalent to a `Vec<TokenTree>`. A `TokenTree` is very similar to the `tt` (Token Tree) metavariable type used in `macro_rules!`, with only a few minor differences.

## See TokenStream in Action

```rust,editable,compile_fail
use quote::quote;

fn main() {
    // Convert Rust code to a TokenStream.
    let token_stream = quote! {
        // Comments and whitespace are ignored.

        //! inner doc comment
        // Note: `//! inner doc comment` is parsed as `#![doc = " inner doc comment"]`

        /// doc comment
        // Note: `/// doc comment` is parsed as `#[doc = " doc comment"]`
        fn print_sum(a: i32, b: i32) {
            println!("{}", a + b);
        }
    };

    println!("{}\n", token_stream.to_string());

    for (i, tt) in token_stream.clone().into_iter().enumerate() {
        println!("token {}:", i);
        println!("source code: {}", tt);
        println!("TokenTree: {:?}\n", tt);
    }
}
```

> [!TIP]
> We'll talk about `quote!` in detail in the [quote](./quote.md) chapter.
