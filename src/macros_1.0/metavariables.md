# Metavariables

Without metavariables, a rule can only match literal values exactly.

## A First Look at Metavariables

```rust,editable
macro_rules! print{
    ($x:tt) => { println!("{}", $x); };
}
fn main() {
    print!(1);
    let v="Hello, world!";
    print!(v);
}
```

`$x:tt` is a metavariable named `x` with the type `tt` (token tree).

> [!NOTE]
> You may find that other resources use the term "fragment specifier" to describe metavariable types.

`tt` (Token Tree): Matches a single token or multiple tokens within matching delimiters `()`, `[]`, or `{}`.

While there are many other metavariable types, the `tt` type is the most flexible. You can think of it as a "catch-all" metavariable type.

## Building a DSL Example

```rust,editable
macro_rules! SQL{
// `expr` matches an expression.
(SELECT $e:expr;)=>{
      println!("Value: {}", $e);
};

// `ident` matches a single identifier.
(SELECT * FROM $t:ident)=>{
      println!("Table: {}", $t);
};

// Catch-all case. This uses repetition, which will be introduced in the next chapter.
($($t:tt)*) => {
      print!("Unknown: ");
      $(
          print!("{} ", stringify!($t));
      )*
      println!();
  };
}
fn main(){
    SQL!(SELECT (3+2*4););
    let user_table="USER_TABLE";
    SQL!(SELECT * FROM user_table);
    SQL!(SELECT 1+1 AS total FROM "USER_TABLE" WHERE id > 10);
}
```

> [!TIP]
> Always use the most specific metavariable type possible. For example, if you only need to match a single identifier, use `ident` instead of `tt`.

## The Built-in `$crate` Metavariable

`$crate` is a built-in metavariable that always expands to the name of the crate being compiled.

```rust,editable
mod utils{
    pub const message: &'static str="Hello, world!";
}
macro_rules! greet{
    () => {
        println!("{}!", $crate::utils::message);
    };
}
fn main(){
    greet!();
}
```

## Metavariable Types

| Type | Description | Example |
| :--- | :--- | :--- |
| `block` | A block expression (code enclosed in braces `{}`). | `{ let x = 5; x }` |
| `expr` | A valid Rust expression. (In Rust 2024, this includes `_` and `const {}`). | `2 + 2`, `f(x)`, `const { 1 + 1 }` |
| `expr_2021` | An expression, but excludes `_` (UnderscoreExpression) and `const {}` (ConstBlockExpression). | `a * b`, `Some(42)` |
| `ident` | An identifier or a keyword. (Excludes `_`, raw identifiers like `r#foo`, and `$crate`). | `count`, `String`, `match` |
| `item` | A Rust item (such as a function, struct, trait, or module). | `fn run() {}`, `struct User { id: u32 }` |
| `lifetime` | A lifetime token. | `'a`, `'static` |
| `literal` | A literal value (optionally prefixed with a minus sign `-`). | `42`, `-10.5`, `"hello"`, `b'a'` |
| `meta` | The contents (the "meta item") found inside an attribute like `#[...]`. | `derive(Debug, Clone)`, `name = "value"` |
| `pat` | A pattern. Since Rust 2021, it allows top-level "or-patterns" (using `\|`). | `Some(x)`, `1..=5`, `_`, `0 \| 1` |
| `pat_param` | A pattern that does **not** allow top-level "or-patterns". | `Some(x)`, `42` |
| `path` | A path (TypePath style). | `std::io`, `self::utils::process` |
| `stmt` | A statement. Usually captures without the trailing semicolon. | `let x = 1`, `drop(y)` |
| `tt` | A **Token Tree**. It can be a single token or tokens inside matching `()`, `[]`, or `{}`. | `!`, `my_var`, `{ 1, 2, 3 }` |
| `ty` | A Rust type. | `i32`, `Vec<String>`, `&'a str` |
| `vis` | A visibility qualifier (this can be empty). | `pub`, `pub(crate)` |
