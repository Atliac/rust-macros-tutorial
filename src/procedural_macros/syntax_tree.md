# Syntax Tree

A syntax tree is a hierarchical representation of source code. It transforms a flat, linear stream of tokens into a structured format that is easy to process and manipulate. The `syn` crate provides a complete syntax tree that can represent any valid Rust source code. We can use `syn` to define our own syntax trees; for example, we can define a syntax tree for HTML, CSS, or any other DSL.

A syntax tree is made up of syntax tree nodes. A syntax tree node can be a token, a group of tokens, or a value of a type that implements the `syn::parse::Parse` trait.

## See a Syntax Tree Node in Action

`syn::File` is a syntax tree (root) node that represents a full source file.

```rust,editable,compile_fail
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
> Don't worry if the output seems overwhelming. You don't need to understand it unless you are working with a full Rust source file.
>
> Furthermore, we won't be using `syn::File` in this tutorial.

We will learn how to define our own syntax tree nodes. But first, let's explore some basic parsing techniques.

## Parsing a Single Token

### Token!

[Token!](https://docs.rs/syn/latest/syn/macro.Token.html) is a type macro that expands to the Rust type representing a specific token.

```rust,editable,compile_fail
use syn::*;

fn main() {
    // Parse the `pub` keyword
    let input = quote::quote! {pub};
    let _token: Token![pub] = parse2(input).unwrap();
    // Or use parse_quote!
    let _token: Token![pub] = parse_quote! {pub};

    // Parse the `struct` keyword
    let _token: Token![struct] = parse_quote! {struct};

    // Parse `+=`
    let _token: Token![+=] = parse_quote! {+=};

    // Parse `::`
    let _token: Token![::] = parse_quote! {::};

    // Error: `pub fn main() {}` is not a single token
    // let _token: Token![pub] = parse_quote! {pub fn main() {}};
}
```

### custom_keyword!

```rust,editable,compile_fail
use syn::*;

// We define custom keywords in a `kw` or `keywords` module by convention.
mod kw{
    syn::custom_keyword!(div);
}

fn main() {
    let _token: kw::div = parse_quote! {div};
}
```

## Parsing a Syntax Tree Node

```rust,editable,compile_fail
use syn::*;

fn main() {
    let _node: ItemFn = parse_quote! {fn main() {println!("Hello, world!")}};
    let _node: ItemStruct = parse_quote! {struct MyStruct {field: i32}};
    // `syn::DeriveInput` is a syntax tree node that represents any valid input to a derive macro.
    let _node: DeriveInput = parse_quote! {#[derive(Debug)] struct MyStruct {field: i32}};
}
```

## Parsing a Custom Syntax Tree Node

There are two ways to parse a custom syntax tree node:

1. Use a function or closure.
2. Define a custom syntax tree node type that implements the `syn::parse::Parse` trait.

### Using a function or closure

```rust,editable,compile_fail
use quote::*;
use syn::{
    parse::{ParseStream, Parser},
    *,
};

fn main() {
    let input = quote! {
        <div>"Hello World"</div>
    };
    // parse::Parser::parse2(|input: ParseStream| -> Result<()> { todo!() }, input).unwrap();
    // or
    let parser = |input: ParseStream| -> Result<()> {
        // `ParseStream::parse()` parses a syntax tree node of type `T`,
        // advancing the cursor of the parse stream past it.

        // `<`
        input.parse::<Token![<]>()?;
        // `div`
        input.parse::<Ident>()?;
        // `>`
        input.parse::<Token![>]>()?;
        // `"Hello World"`
        input.parse::<LitStr>()?;
        // `<`
        input.parse::<Token![<]>()?;
        // `/`
        input.parse::<Token![/]>()?;
        // `div`
        input.parse::<Ident>()?;
        // `>`
        input.parse::<Token![>]>()?;
        Ok(())
    };
    parser.parse2(input).unwrap();
}
```

### Defining a custom syntax tree node type by implementing the `syn::parse::Parse` trait

```rust,ignore
struct HtmlNode{...}
impl Parse for HtmlNode{
    fn parse(input: ParseStream) -> Result<Self> {
        todo!()
    }
}
fn main(){
    let node: HtmlNode = parse_quote!{
        <div>"Hello World"</div>
    };
}
```

> [!TIP]
> Complex tree nodes (such as `syn::File`) are composed of simpler tree nodes.
>
> I hope this gives you a clear idea of how to define a custom syntax tree, even for more complex structures.
