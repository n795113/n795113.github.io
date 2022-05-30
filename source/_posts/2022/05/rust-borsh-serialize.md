---
title: "Rust: Borsh Serialize"
date: 2022-05-23 22:13:57
categories: [Tech, Rust]
tags:
  - rust
  - solana
---
Study how to use Borsh Serialize. When we write vanila Solana program, we need to serialize and deserialize accounts' data by ourselves.

```rust
// Cargo.toml
// ...
[dependencies]
borsh = "0.9.3"
borsh-derive = "0.9.1"
```

```rust
// main.rs
use borsh::{BorshDeserialize, BorshSerialize};

#[derive(BorshSerialize, BorshDeserialize, Debug)]
struct Node {
    num: i32,
    str: String
}

fn main() {
    let uint:i32 = 256; // 2^8 -> 1 byte
    let node = Node{
        num: uint.pow(0) + uint.pow(1)*2 + uint.pow(2)*3 + uint.pow(3)*4, // [1, 2, 3, 4]
        str: String::from("好") // first 4 bytes represents how many bytes of this string
    };

    if let Ok(buf) = node.try_to_vec() {
        println!("serialized: {:?}", buf);

        if let Ok(deserz_node) = Node::try_from_slice(&buf) {
            println!("deserialized: {:?}", deserz_node);
        }
    }
}
```

Run the code:
```
$ cargo run
serialized: [1, 2, 3, 4, 3, 0, 0, 0, 229, 165, 189]
deserialized: Node { num: 67305985, str: "好" }
```