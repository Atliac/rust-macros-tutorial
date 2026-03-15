# Macros 2.0

[#39412](https://github.com/rust-lang/rust/issues/39412)

> [!NOTE]
> Macros 2.0 is a proposal to improve the macros 1.0 (declarative macros `macro_rules!`). It is not yet implemented.

## Why Macros 2.0?

Macros 1.0 has several limitations:

- Lack of hygiene: Macros 1.0 can capture variables from the surrounding scope, which can lead to unexpected behavior.
- Lack of modularity: Macros 1.0 cannot be easily composed or reused.
- Lack of type safety: Macros 1.0 cannot be easily checked for type safety.

Macros 2.0 aims to address these limitations by providing a more powerful and flexible macro system.

> [!NOTE]
> We will get it back when it is implemented.

> [!TIP]
> We actually don't need neither macros 1.0 nor macros 2.0, if we can use procedural macros.
