# Introduction

Welcome Rustacean! Let's learn about Rust macros.

## Prerequisites

To get the most out of this tutorial, we recommend having:

- **Familiarity with basic Rust syntax** (variables, functions, and structs).
- **A functional Rust development environment** (with `cargo` installed).
- **A solid understanding** of Rust's core concepts, such as the type system and ownership.

If you're new to Rust, consider completing the official [Rust Book](https://doc.rust-lang.org/book/) first.

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

Here's what you'll learn in this tutorial:
- How to write your own macros
- Best practices for macro safety and maintainability
- Common macro patterns in the Rust ecosystem

## How to Use This Book

This tutorial is designed to be interactive and hands-on. Here's how to get the most out of it:

### Reading Order

For the best learning experience, we recommend reading this tutorial in order. While you are free to explore different sections, macros build on concepts progressively; following the intended sequence will help you understand the material more effectively.

### Interactive Code Blocks

This book contains many **runnable and editable** Rust code blocks:

- **Runnable**: Click the play button <i class="fas fa-play"></i> at the top-right of any code block to execute it and see the output directly.
- **Editable**: Edit the code directly in the editor, then click the play button to re-run the updated code with your changes.


### Tips for Learning

- Try modifying the examples to understand how macros work
- Use the <i class="fas fa-play"></i> button to verify your changes compile and behave as expected
- Don't hesitate to break things - the tutorial is designed for experimentation
