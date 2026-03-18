# Procedural Macro Crates

Congratulations, fellow Rustacean🦀! You've reached a major milestone. Now, you're ready to learn the final, essential step: how to package and use your procedural macros in a crate.

## Creating a Proc-Macro Crate

A dedicated library crate is mandatory for procedural macros because they must have the `proc-macro` crate type enabled. Crucially, they cannot be defined within the same crate that uses them.

To create one, run:

```bash
cargo new --lib my_proc_macros
cd my_proc_macros
cargo add syn quote proc-macro2
```

Then, update your `Cargo.toml` to enable the `proc-macro` feature:

```toml
[lib]
proc-macro = true
```

## Function-like Procedural Macros

Function-like macros are called with a following exclamation mark (like `println!`).

```rust,ignore
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, Ident};

// In `src/lib.rs`, define a function-like procedural macro.
#[proc_macro]
pub fn hello_macro(input: TokenStream) -> TokenStream {
    // Parse the input tokens into a syn Ident
    let name = parse_macro_input!(input as Ident);
    let name_str = name.to_string();

    let output = quote! {
        println!("Hello, {}!", #name_str);
    };

    TokenStream::from(output)
}
```

We can now use `hello_macro` in a normal crate:

```rust,ignore
use my_proc_macros::hello_macro;

fn main() {
    hello_macro!(world);
}
```

### The `input` Parameter
The `input` contains the tokens enclosed by whatever delimiters (parentheses `()`, brackets `[]`, or braces `{}`) are used when calling the macro. In the example above, the input is `world`. Upon expansion, the call `hello_macro!(world)` is replaced entirely by the macro's `output`.

### The `parse_macro_input!` Macro
The `parse_macro_input!` macro parses the input `TokenStream` into a specific `syn` syntax tree node. If parsing fails, it automatically generates a high-quality compile-time error.

The basic syntax is `parse_macro_input!(<TokenStream> as <Type>)`. This convenience macro is specifically designed for use within `proc-macro` crates.

## Attribute Procedural Macros

Attribute macros define custom attributes that can be attached to items like functions or structs.

```rust,ignore
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, ItemFn};

// In `src/lib.rs`, define an attribute procedural macro.
#[proc_macro_attribute]
pub fn my_attribute(_attr: TokenStream, item: TokenStream) -> TokenStream {
    // We parse the item (e.g., a function) the attribute is attached to
    let input_item = parse_macro_input!(item as ItemFn);

    // We keep the original item as-is
    let output = quote! {
        #input_item
    };

    TokenStream::from(output)
}
```

We can use `my_attribute` in a normal crate:

```rust,ignore
use my_proc_macros::my_attribute;

#[my_attribute(attr1, attr2, key=value)]
pub fn foo() {
    println!("Hello from foo!");
}
```

### Parameters in Attribute Macros
- **`attr`**: The tokens inside the attribute's parentheses (e.g., `attr1, attr2`).
- **`item`**: The tokens for the item the attribute is attached to (e.g., a function, struct, or enum).

Unlike function-like macros, which replace the call itself, an attribute macro replaces the **entire item** it is attached to with its `output`.

## Derive Procedural Macros

Derive macros create new items (usually trait implementations) for an existing item.

```rust,ignore
use proc_macro::TokenStream;
use quote::quote;
use syn::{parse_macro_input, DeriveInput};

#[proc_macro_derive(MyDerive, attributes(my_helper))]
pub fn my_derive(input: TokenStream) -> TokenStream {
    // Parse the entire struct/enum/union
    let input = parse_macro_input!(input as DeriveInput);
    let name = input.ident;

    let output = quote! {
        impl MyTrait for #name {
            fn hello() {
                println!("Hello from my derive!");
            }
        }
    };

    TokenStream::from(output)
}
```

We can use `MyDerive` in a normal crate:

```rust,ignore
use my_proc_macros::MyDerive;

#[derive(MyDerive)]
pub struct MyStruct {
    #[my_helper]
    pub field1: i32,
}
```

### The `input` Parameter
The `input` TokenStream represents the entire item (struct, enum, or union) that the `#[derive(...)]` attribute is decorating.

### Append-only Expansion
Unlike attribute macros, a derive macro's output does **not** replace the input item. Instead, the output (usually an implementation block) is **appended** to the module or block containing the original item.

---

*Happy macro programming!* 🦀
