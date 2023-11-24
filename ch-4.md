# Chapter 4. Understanding Ownership

## Stack and the Heap

1. Data is stored in the stack in the FIFO order.
1. All data stored on the stack must have a known, fixed size. 
1. Data with an unknown size at compile time or a size that may change must be
   stored on the heap instead.
1. When you want to put something to a heap, memory allocator finds an empty
   spot in the heap that's big enough, marks it as in use and returns the
   pointer to that location. This process is called allocation. 

## Ownership Rules

1. Each value has an owner.
1. There can only be one owner at a time.
1. When the owner goes out of scope, the value will be dropped.

## String
1. `String` can be mutated but literals cannot.
1. Literal is stored in the stack, the `String` type on the heap.

```rust
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid
```

```rust
// String is created on the heap,
// s1 is a fixed sized type that holds capacity of the string, its size and the
// pointer to the heap
let s1 = String::from("hello");

// The following doesn't create a copy of the string in heap. 
// Instead s2 refers to the same string type in the heap
let s2 = s1;
// But this means that when s1 and s2 go out of scope, trying to free both of
// them frees memory that's already free. This is a *double free* error.
// "To ensure memory safety, after the line `let s2 = s1;`, Rust considers
// `s1` as no longer valid. Therefore, Rust doesn't need to free anything when 
// `s1` goes out of scope.
```

1. Something that looks like shallow copy in the line `let s2 = s1;`,  is actually a **move**. It's called so because in addition to making a shallow copy, variable `s1` is invalidated.
1. Rust never automatically creates "deep" copy of objects in the heap.
1. Rust creates "deep" copy of data stored in the stack. In fact, there is no
   idea of shallow copy for data in stack. For example:
```rust
let x = 5;
let y = x;
println!("x= {}, y = {}", x, y)
```
1. "The mechanics of passing a value to a function are similar to those when assigning a value to a variable. Passing a variable to a function will move or copy, just as assignment does. Listing 4-3 has an example with some annotations showing where variables go into and out of scope."

```rust
// Listing 4.3
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

1. Returning values can also transfer ownership.

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

1. Object isn't **moved** when a variable reference to a varible is passed.
1. Unlike in Go and C, the poiter type is denoted by & not *.
1. Like Go and C, & is the reference operator.
1. If you have a mutable reference to a value, you can have no other references to tvalue.
1. The act of creating a reference is called "borrowing".
1. Rust can prevent data races at compile time.
1. We cannot have a mutable reference while we have an immutable on the same value.
1. Multiple immutable references are allowed. This is safe.

```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // Compiler can infer that:
    // variables r1 and r2 will not be used after this point
    // Thus their scope can be said to end at this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```

1. The scopes of the immutable references end where they are last used. 

### Rules of reference
1. At any given time, you can have either one mutable reference or any number of immutable references.
1. References must always be valid.
