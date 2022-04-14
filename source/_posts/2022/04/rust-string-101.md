---
title: "Rust: String 101"
permalink: rust-string-101
toc: false
date: 2022-04-13 23:28:05
categories: [Tech, Rust]
tags:
    - rust
---
```rust
fn main() {
    // let mut str = "Hello "; <- this won't work
    let mut str = String::from("Hello ");
    println!("Now str: {}", str);
    
    // Get length & capacity of a string
    println!("Length: {} Capacity: {}", str.len(), str.capacity());

    // Push char
    str.push('R');
    println!("Now str: {}", str);

    // Push string
    str.push_str("ust"); // capacity is doubled to 12
    println!("Now str: {}", str);

    // Check if empty
    println!("Is empty: {}", str.is_empty());

    // Check if contains
    println!("Contains 'PHP'? {}", str.contains("PHP"));

    // Replace substring with
    println!("Replace output: {}", str.replace("Rust", "Solana"));
    // replace is not a in-place operation
    // so value of str is still "Hello Rust"

    // Iterate a string by word (whitespace)
    for word in str.split_whitespace() {
        println!("{}", word);
    }

    // Create a string with capacity
    let str_with_cap_10 = String::with_capacity(10);
    println!("Length: {} Capacity: {}", str_with_cap_10.len(), str_with_cap_10.capacity());
}
```

The above outputs the following:
```
Now str: Hello 
Length: 6 Capacity: 6
Now str: Hello R
Now str: Hello Rust
Is empty: false
Contains 'PHP'? false
Replace output: Hello Solana
Hello
Rust
Length: 0 Capacity: 10
```