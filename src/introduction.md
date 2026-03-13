# Introduction

Welcome Rustacean! Let's learn about Rust macros.

## What are Macros

Simply put, macros are a metaprogramming language that allows us to write code that generates code.

They are especially useful when you need:
- Repeatable code patterns
- Domain-specific abstractions
- Compile-time computations

## Types of Macros

Rust has two main kinds of macros:

1. Declarative macros: defined once, expanded many times
    1. [macros 1.0](./macros_1.0.md) (`macro_rules!`): [Macros by Example](https://doc.rust-lang.org/reference/macros-by-example.html) define new syntax in a higher-level, declarative way.
    2. macros 2.0: "[Macros 2.0 #39412](https://github.com/rust-lang/rust/issues/39412)" is a general term for a long-running, experimental effort within the Rust project to create a unified and improved macro system to replace the original `macro_rules!` system. It is not a feature currently available on stable Rust.
2. [Procedural macros](./procedural_macros.md): Procedural macros allow creating syntax extensions as execution of a function. Procedural macros come in one of three flavors:
   - Function-like macros - `custom!(...)`
   - Derive macros - `#[derive(CustomDerive)]`
   - Attribute macros - `#[CustomAttribute]`

## What You'll Learn

In this tutorial, you will learn:
- How macros work under the hood
- How to write your own macros
- Best practices for macro safety and maintainability
- Common macro patterns used in the Rust ecosystem

## How to Use This Book

It is recommended to read the first part ("Tutorial") in order.

These books may contain many runnable code blocks. It is encouraged to run them by clicking the <i class="fas fa-play"></i> button at the top-right of each code block.

For example, there is a runnable example below:

```rust
fn main() {
    println!("Hello, world!");
}
```
