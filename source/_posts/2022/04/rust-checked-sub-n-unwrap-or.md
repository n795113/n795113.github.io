---
title: "Rust: checked_sub & unwrap_or"
permalink: rust_checked_sub_n_unwrap_or
toc: false
date: 2022-04-28 00:34:33
categories: [Tech, Rust]
tags:
  - rust
---

Just a quick note for these two functions I learned today.

`checked_sub` is used to check if the abstraction of an integer overflows. For example:

```rust
let mut num_u32: u32 = 0;
num_u32 -= 1; // overflow

let mut num_i32 = i32::MIN; // -2147483648
num_i32 -= 1; // overflow
```

To prevent it, we can use the combo of `checked_sub` and `unwrap_or`. `checked_sub` will return `Option<self>` which means if there is no overflow occurs, we get `Some(substraction_result)`, else we will get `None`. `unwarp_or` can then give the result a default value if we get `None` or just unwrap `Some(substraction_result)` to return `substraction_result`. Let's look at the code.

```rust=
let mut num_i32 = i32::MIN + 1; // -2147483648 + 1

num_i32 = num_i32.checked_sub(1).unwrap_or(i32::MIN); // -2147483648

num_i32 = num_i32.checked_sub(1).unwrap_or(100); // 100
// Since it overflows, it will return the default value 
// that we give to unwrap_or which is 100 for this example
```

That's it for today!