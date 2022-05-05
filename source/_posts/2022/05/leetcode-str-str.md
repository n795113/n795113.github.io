---
title: "Solve Leetcode in Rust: Implement strStr()"
date: 2022-05-05 12:10:55
categories: [Tech, Rust]
tags:
  - leetcode
  - rust
  - hash
---
## Problem
Problem: [Implement strStr()](https://leetcode.com/problems/implement-strstr)

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack. If needle is empty, return 0.

## Thoughts

strategies:
- two pointers to compare each characters of needle and haystack (2 while loops)
- iterate through haystack to get slices to compare with needle
- iterate through haystack to get slices. Hash each slice and compare the hashed with the hashed needle
- use `find` method


## Code

### Two While Loops
```rust
impl Solution {
    pub fn str_str(haystack: String, needle: String) -> i32 {
        let hay_vec: Vec<char> = haystack.chars().collect();
        let needle_vec: Vec<char> = needle.chars().collect();

        let mut i_hay: usize = 0;
        let mut i_needle: usize = 0;
        while i_hay < hay_vec.len() {
            while i_needle < needle_vec.len() {

                let offset_hay = i_hay + i_needle;
                if offset_hay >= hay_vec.len() {
                    break;
                }

                if hay_vec[offset_hay] != needle_vec[i_needle] {
                    i_needle = 0;
                    break;
                } else if hay_vec[offset_hay] == needle_vec[i_needle] && i_needle == needle_vec.len() - 1 {
                    return i_hay as i32;
                }

                i_needle += 1;
            }

            i_hay += 1;
        }

        return -1;
    }
}
```

### Compare String Slice
```rust
impl Solution {
    pub fn str_str(haystack: String, needle: String) -> i32 {
        let (hay_len, needle_len) = (haystack.len(), needle.len());

        if needle_len == 0 { return 0 }
        else if needle_len > hay_len { return -1 } // avoid hay: "aa", needle: "aaaa"

        for i in 0..=hay_len - needle_len {
            if haystack[i..i + needle_len] == needle {
                return i as i32;
            }
        }

        -1
    }
}
```

### Use Manual Hash
```rust
const MAGIC_NUM: u64 = 31; // can be any number but should not too big

pub fn str_str(haystack: String, needle: String) -> i32 {
    let (hay_len, needle_len) = (haystack.len(), needle.len());

    if needle_len == 0 { return 0 }
    else if needle_len > hay_len { return -1 } // avoid hay: "aa", needle: "aaaa"

    let hash_needle = hash(&needle);
    let mut hash_hay:u64 = 0;
    let highest_pow = MAGIC_NUM.pow(needle_len as u32 - 1);

    for i in 0..=hay_len - needle_len {
        if hash_hay == 0 { 
            hash_hay = hash(&haystack[i..i + needle_len]); 
        } else {
            /* rolling hash */ 
            // abc -> _bc
            let first_c = haystack.chars().nth(i-1).unwrap();
            hash_hay -= first_c as u64 * highest_pow;
            // _bc -> bc_
            hash_hay *= MAGIC_NUM;
            // bc_ -> bcd
            hash_hay += haystack.chars().nth(i + needle_len - 1).unwrap() as u64;
        }

        if hash_hay == hash_needle {
            return i as i32;
        }
    }

    -1
}

fn hash(str: &str) -> u64 {
    let mut buf:u64 = 0;

    for c in str.chars() {
        buf *= MAGIC_NUM;
        buf += c as u64;
    }

    buf
}
```

### Use Find
```rust
impl Solution {
    pub fn str_str(haystack: String, needle: String) -> i32 {
        if needle.is_empty() { return 0 }

        match &haystack.find(&needle) {
            Some(i) => *i as i32,
            None => -1
        }
    }
}
```

## If An Unicode String Given

```rust
let str = String::from("好");
println!("{}", str.len()); // This will print out 3
```

Since the `len()` returns the length of bytes not characters, we need to change our code if unicode strings given.

```rust
impl Solution {
    pub fn str_str(haystack: String, needle: String) -> i32 {
        // Get length of chars instead of bytes
        let (hay_len, needle_len) = (
            haystack.chars().count(),
            needle.chars().count()
        );

        // Collect string into `Vec<char>`
        let (hay_chars, needle_chars) = (
            haystack.chars().collect::<Vec<char>>(),
            needle.chars().collect::<Vec<char>>()
        );

        // The rest are same
        if needle_len == 0 { return 0 }
        else if needle_len > hay_len { return -1 }

        for i in 0..=hay_len - needle_len {
            if hay_chars[i..i + needle_len] == needle_chars {
                return i as i32;
            }
        }

        -1
    }
}
```

## Conculsion
- utilize string slice or `find` looks easier.
- in some situations, hash may be useful. However, for this case, it may not be the best practise.