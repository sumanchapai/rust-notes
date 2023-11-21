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
1. xxx
