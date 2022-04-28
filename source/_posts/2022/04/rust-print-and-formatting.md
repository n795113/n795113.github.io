---
title: "Rust: Print & Formatting"
date: 2022-04-13 22:39:22
categories: [Tech, Rust]
tags:
    - rust
---

List some ways to format the value to be printable.

```rust
fn main() {
    // Print to console
    println!("Hello Rust");

    // Basic Formatting
    println!("{} is {}", "Rust", "rusty");

    // Positional arguments
    println!(
        "{0} is a {1}, and {0} is {2}",
        "Rust", "programming language", "efficient"
    );

    // Named arguments
    println!(
        "{target} supports {noun}",
        target = "Rust",
        noun = "concurrent programming"
    );

    // Placeholder traits
    println!("Binary: {:b} Hex: {:x} Octal: {:o}", 10, 10, 10);

    // Placeholder for debug trait
    println!("{:?}", (100, false, "Rust"));
}
```

The above outputs the following:
```
Hello Rust
Rust is rusty
Rust is a programming language, and Rust is efficient
Rust supports concurrent programming
Binary: 1010 Hex: a Octal: 12
(100, false, "Rust")
```