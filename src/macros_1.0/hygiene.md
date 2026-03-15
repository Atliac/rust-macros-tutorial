# Hygiene

```rust,editable

fn func() {
    println!("Hello, world! (from definition site)");
}

fn main(){
    let x = 1;
    macro_rules! m {
        () => {
            println!("x = {}", x); // Uses `x` from the definition site.
            func(); // Uses `func` from the invocation site.
            $crate::func(); // meta-variable `$crate` refers to the crate where the macro is defined.
        };
    }
    {
        let x = 2;
        fn func() {
            println!("Hello, Rustacean! (from invocation site)");
        }
        m!();
    }
}
```
