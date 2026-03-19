# Repetitions

We use `$(<matcher | transcriber>)[delimiter]<*|?|+>` to specify metavariable repetition in both the pattern (matcher) and the expansion (transcriber).

- `*`: Zero or more times
- `?`: Zero or one time
- `+`: One or more times

## Optional Repetitions (?)

```rust,editable
macro_rules! greet{
  ($($msg:literal)?) => {
    let msg = concat!("Hello, world! ", $($msg)?);
    println!("{}", msg);
  }
}

fn main(){
  greet!();
  greet!("Hello, Rustacean! 👋");
}
```

## Zero or More Repetitions (*)

```rust,editable
macro_rules! sum{
  ($($n:expr)*) => {
      let mut sum=0;
      $(sum += $n;)*
      println!("The sum is {}", sum);
  }
}
fn main(){
  sum!(1 2 3);
}
```

## Repetitions with Delimiters

```rust,editable
macro_rules! sum{
  ($($n:expr),*) => {
      let mut sum=0;
      $(sum += $n;)*
      println!("The sum is {}", sum);
  }
}
fn main(){
  sum!(1, 2, 3);
}
```

## Custom Delimiter Tokens

```rust,editable
macro_rules! sum{
  ($($n:literal)add*) => {
      let mut sum=0;
      $(sum += $n;)*
      println!("The sum is {}", sum);
  }
}
fn main(){
  sum!(1 add 2 add 3);
}
```

> [!TIP]
> The delimiter token can be any token other than a delimiter or one of the repetition operators, but `;` and `,` are the most common.
