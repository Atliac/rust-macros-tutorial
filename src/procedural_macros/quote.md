# The `quote` Crate

We have discussed how to parse input using `syn`. Now, it's time to generate output with `quote`.

## quote!

The `quote!` macro is a procedural macro that takes a template of Rust code and returns a `TokenStream`. Its usage is similar to `macro_rules!`.

|Macro Name|Interpolation|Interpolation Type|Repetition|
|:---|:---|:---|:---|
|macro_rules!|`$var`|metavariable|`$(<...>)[delimiter]<*\|?\|+>`|
|quote!|`#var`|any type implementing `ToTokens`|`#(<...>)[delimiter]*`|

```rust,editable
fn main() {
    let f: syn::ItemFn = syn::parse_quote!(
        fn foo(x: i32) -> i32 {
            println!("Hello World");
        }
    );
    let fn_name = f.sig.ident;
    let fn_input = f.sig.inputs;
    let fn_output = f.sig.output;
    let fn_block = f.block;
    // Convert the function to be public
    let token_stream = quote::quote! {
        pub fn #fn_name (#fn_input) #fn_output #fn_block
    };
    println!("{}", token_stream);
}
```
