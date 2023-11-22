# [Chapter 3](https://doc.rust-lang.org/book/ch03-00-common-programming-concepts.html)

## 3.1 Variables and Mutability
1. Variables are immutable by default
1. Constants are compile time values. Compiler can do things like simple arithmetic at compile time.
1. Redeclaring a variable is legal. Most recent declaration shadows the previous ones.

```rust
// Note this is legal. 
// x is immutable, we're creating new variables each time we use let
// This is different from making x mutable
let x = 5;
let x = 6
```

```rust
// Illegal because variables are immutable by default
let x = 5;
x = 6;
```

## 3.2 Data Types
1. Data Type means R-Type which means the type of a **value**.
1. Variables must be type annotated when it's otherwise ambiguous like parsing string to number.
1. Scalars: integers (i for unsigned and u for signed), floats (f32, f64), bool, char
1. Compound: tuples and arrays



## 3.2 Functions
1. Use snake_case names.
1. Place functions anywhere in a file. Definition doesn't need to placed strictly ebfore use.
1. What's inside a block scope `{}` is an expression.
1. Assignment is a statment, not an expression.
1. Expressions don't have semicolons at the end.
1. A expression with a semicolon at the end is a statement.


```rust
// This is legal since a block scope is an expression
let y = {
    let x = 3;
    x + 1
};
// At this point, y is an immutable variable whose R-Value is 4
// Note that there's no semicolon after x+1 but after the closing }
```

## 3.3 Comments
1. // marks beginning of an end of line comment.

## 3.4 Control flow
1. The condition in the if block must be a `bool`.
1. `if` is an expression not a statement.
1. A block of code evaluates to the last expression in it. For example 
```rust
let x = {
    5 + 5;
    1 * 10;
    1 + 2
};
// Here the value of the block is the value of the last expression i.e 3.
// Thus R-Type of x is 3
```
1. `if` and the `else` arms must return values of compatible types. Perhaps the formal definition of compatibility is more compelx but general rules apply. For example integer and string types are not compatible.
1. Three kinds of loops: `loop`, `while` and `for`

