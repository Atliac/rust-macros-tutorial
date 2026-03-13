# Macros 1.0: macro_rules!

`macro_rules!` is a special macro that allows us to define our own macros in a declarative way. We call such extensions “macros by example” or simply “macros”.

> [!NOTE]
> We will use `macro` to refer to "macro by example", which is defined by `macro_rules!` throughout this chapter.

## Hello World

```rust,editable
fn main(){
    macro_rules! hello_world {
        () => {
            println!("Hello, world!");
        };
    }
    hello_world!();
}
```
> [!NOTE]
> With `macro_rules!`, you can use `()`, `[]`, or `{}` interchangeably in both the macro declaration and the macro call. For example, `hello_world!()` is equivalent to `hello_world![]` and `hello_world!{}`.

## Macro Components Explained

```rust,ignore
macro_rules! <macro_name> {
    (<matcher_of_first_rule>) => {
        <transcriber_of_first_rule>
    };
    
    (matcher_of_second_rule>) => {
        <transcriber_of_second_rule>
    };
    ...
}
```

1. Each macro has a name, and one or more rules.
2. Each rule has two parts: 
    - a matcher, describing the syntax that it matches
    - a transcriber, describing the syntax that will replace a successfully matched invocation.
3. Macros can expand to expressions, statements, items (including traits, impls, and foreign items), types, or patterns.

## Multi-rules Example

```rust, editable
macro_rules! echo{
  (Hello World) => {
      println!("Rule 1: Hello World");
  };
  
  (Hello) => {
      println!("Rule 2: Hello");
  };
  
  // use a metavariable to catch all
  // we will introduce metavariables later
  ($($name:tt)*) => {
      print!("Rule 3: ");
      $(
          print!("{}", stringify!($name));          
      )*    
      println!();
  };
}

fn main(){
    echo!(Hello World);
    echo!(Hello);
    echo!(3 + 2 = 5!);
    echo!(1,2,3);
}
```
