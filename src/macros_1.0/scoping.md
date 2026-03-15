# Scoping

## Textual scope

```rust,editable
fn main(){
    // m!{}; // Error: macro `m` is not defined in this scope

    // new scope
    {
        macro_rules! m{
            () => {
                println!("Hello, world!");
            }
        }
        m!{}; // OK

        // shadowing
        macro_rules! m{
            () => {
                println!("Hello, Rustacean!");
            }
        }
        m!{}; // OK
    }

    // m!{}; // Error: macro `m` is not defined in this scope
}
```

> [!TIP]
> Textual scope works similarly to the scope of local variables declared with `let`.

## Path-based Scope

```rust,editable
mod mod1{
    macro_rules! m{
        () => {
            println!("Hello, world!");
        }
    }
}

mod mod2{
    macro_rules! m{
        () => {
            println!("Hello, Rustacean!");
        }
    }
    pub(crate) use m; // re-export to gain path-based scope
}

fn main(){
    // mod1::m!{}; // Error: By default, a macro has no path-based scope.
    mod2::m!{}; // OK: `m` is re-exported from `mod2`.
}
```

## Exporting Macros

```rust,editable
mod mod_level_1{
    mod mod_level_2{
        // By default, a macro is implicitly `pub(crate)`
        // `#[macro_export]` makes it `pub` and export it to the root of the crate.
        #[macro_export]
        macro_rules! m{
            () => {
                println!("Hello, world!");
            }
        }
    }
}

fn main(){
   // mod_level_1::m!{}; // Error: `m` is not in scope.
   // mod_level_1::mod_level_2::m!{}; // Error: `m` is not in scope.
   crate::m!{}; // OK
}
```
