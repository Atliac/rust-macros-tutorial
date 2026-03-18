# Spans

When parsing malformed input, simply stating what is wrong without indicating its location is not very helpful for debugging.

To provide precise error locations, we use `Span`s. A `Span` is an opaque value representing a specific range of source code. While they cannot be modified, they can be created or retrieved. Their primary purpose is error reporting, and every token carries an associated `Span`.

## Example with Coarse-grained Spans

```rust,compile_fail
# use syn::{
#     parse::{Parse, ParseStream},
#     *,
# };
#
# struct HtmlNode;
# impl Parse for HtmlNode {
#     fn parse(input: ParseStream) -> Result<Self> {
#         input.parse::<Token![<]>()?;
#         input.parse::<Ident>()?;
#         input.parse::<Token![>]>()?;
#         input.parse::<LitStr>()?;
#         input.parse::<Token![<]>()?;
#         input.parse::<Token![/]>()?;
#         input.parse::<Ident>()?;
#         input.parse::<Token![>]>()?;
#         Ok(HtmlNode)
#     }
# }
# fn main() {
    // `quote!` assigns the same span to all tokens inside the block.
    let input = quote::quote! {<div>"Hello World"<div>};
    if let Err(e) = syn::parse2::<HtmlNode>(input) {
        println!("Error: {} at {:?}", e, e.span());
    }
# }
```

## Example with Precise Spans

```rust,editable,compile_fail
use std::str::FromStr;

use proc_macro2::TokenStream;
use syn::{
    parse::{Parse, ParseStream},
    *,
};

struct HtmlNode;
impl Parse for HtmlNode {
    fn parse(input: ParseStream) -> Result<Self> {
        input.parse::<Token![<]>()?;
        input.parse::<Ident>()?;
        input.parse::<Token![>]>()?;
        input.parse::<LitStr>()?;
        input.parse::<Token![<]>()?;
        input.parse::<Token![/]>()?;
        input.parse::<Ident>()?;
        input.parse::<Token![>]>()?;
        Ok(HtmlNode)
    }
}
fn main() {
    // `TokenStream::from_str` assigns a unique span to each individual token.
    let input = TokenStream::from_str(r#"<div>"Hello World"<div>"#).unwrap();
    if let Err(e) = syn::parse2::<HtmlNode>(input) {
        println!("Error: {} at {:?}", e, e.span());
    }
}
```
