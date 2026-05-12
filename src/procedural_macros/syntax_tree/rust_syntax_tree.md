# Rust Syntax Tree

We've learned how to implement our own syntax tree nodes for virtually any DSL. But if we only want to work with valid Rust code using `syn`, we don't need to implement any custom nodes. The good news is that you don't even need to learn anything beyond what this tutorial has already covered about `syn` to handle concrete Rust-code-related tasks.

Here is a simple example to get you started.

# Ok Type Extraction

Let's implement a function that parses a Rust function and extracts the inner success type (Ok type) from its Result return type and prints it to the console.

> [!TIP]
>
> You don't need to read the following implementation. Just run it and observe the output.

```rust,editable,compile_fail
use proc_macro2::TokenStream;
use syn::ItemFn;

fn main() {
    ok_type(syn::parse_quote! {fn foo() -> i32 {}});
    ok_type(syn::parse_quote! {fn foo() -> Result<i32> {}});
    ok_type(syn::parse_quote! {fn add(a:u32,b:u32) -> Result<u32, Error> {}});
}

fn ok_type(item_fn: TokenStream) {
    println!(r#"fn: "{}""#, item_fn.to_string());
    let item_fn: ItemFn = syn::parse_quote! {#item_fn};
    let syn::ReturnType::Type(_, return_type) = item_fn.sig.output else {
        println!("Ok Type Unknown.");
        return;
    };
    let syn::Type::Path(return_type) = return_type.as_ref() else {
        println!("Ok Type Unknown.");
        return;
    };
    let syn::PathArguments::AngleBracketed(generic_args) =
        &return_type.path.segments.first().unwrap().arguments
    else {
        println!("Ok Type Unknown.");
        return;
    };
    let syn::GenericArgument::Type(syn::Type::Path(ok_type)) = generic_args.args.first().unwrap()
    else {
        println!("Ok Type Unknown.");
        return;
    };
    println!("Ok type is: {}", ok_type.path.get_ident().unwrap());
}
```

The above code uses a few `syn` nodes. How do you know in advance which nodes you need to implement your own `ok_type` function? The answer is: no, you don't need to know anything.

You just need to start at the very top node — `ItemFn` for this example. Print its debug output, then figure out the "happy path" that leads to your task target.

For example:

```rust,editable,compile_fail
use syn::ItemFn;

fn main() {
    let item_fn: ItemFn = syn::parse_quote! {fn foo() -> Result<i32> {}};
    println!("{item_fn:#?}");
}
```

Then follow the "happy path" all the way to your target — `i32` in this example. The golden rule is: focus only on the "happy-path".
