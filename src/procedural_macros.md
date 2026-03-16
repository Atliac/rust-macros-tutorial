# Procedural Macros

Procedural macros allow you to run custom code during the compilation process. They operate with the same level of access as the compiler itself, including access to standard I/O streams and the file system. Because of this, procedural macros share the same security considerations as Cargo build scripts. However, they are also subject to several unique constraints:

1. They must be defined in a dedicated crate type specifically marked as a `proc-macro` crate.
2. A `proc-macro` crate can only export procedural macros; it cannot export standard functions or types.
3. Procedural macros cannot be used within the same crate that defines them.
4. Defining them requires the compiler-provided `proc-macro` crate, which is only available within these specialized macro crates.

These constraints can make testing and debugging procedural macros challenging, often creating a barrier for developers who are just starting out.

Fortunately, [David Tolnay](https://github.com/dtolnay), a prominent figure in the Rust community, has created several essential crates that simplify this workflow. In this chapter, we will focus on three of them: [`syn`](https://github.com/dtolnay/syn), [`quote`](https://github.com/dtolnay/quote), and [`proc-macro2`](https://github.com/dtolnay/proc-macro2). These are the standard tools used in nearly all professional Rust projects for building procedural macros.

By using these crates, we can write our macro logic in a standard Rust crate where it can be easily tested and debugged. Once the logic is verified, we can then integrate it into a formal `proc-macro` crate.

Instead of jumping straight into the complexities of procedural macro crates, we will begin our journey by learning how to write "code that generates code" within a standard Rust environment.

Let's go!
