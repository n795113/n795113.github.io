---
title: "Solve Leetcode in Rust: Rebuild A Tree From An Inorder and A Preorder Traversal"
date: 2022-05-05 10:29:21
categories: [Tech, Rust]
tags:
  - leetcode
  - rust
  - bst
---
## Problem
Given a inorder-traversal result and a preorder-traversal result of a tree, Figure out the tree structure.

![](given-inorder-and-preorder.png)

For example:
- inorder: 1,2,3,4,5,6,7
- preorder: 5,2,1,4,3,6,7 

What is the structure of this tree? Can we build a tree based on these two clues?

## Thoughts
The first element of preorder-traversal must be the Root of a tree. On the other hand, we can determine left and right from inorder-traversal after we get the root. As the result, we can again get inorder and preorder result of left-sub-tree and right-sub-tree, then both left and right redo above process until they get no or only one element.

![](process.png)

Overview the tree for now

![](result.png)

So we can expect we can get the whole tree if we repeat the process.

## Code

```rust
pub struct Node {
    pub val: i32,
    pub left: Option<Box<Node>>,
    pub right: Option<Box<Node>>
}

impl Node {
    pub fn new(val: i32) -> Self {
        Node { 
            val,
            left: None,
            right: None
        }
    }

    pub fn inorder_traversal(&self) {
        if let Some(left) = &self.left {
            left.inorder_traversal();
        }

        println!("{}", &self.val);

        if let Some(right) = &self.right {
            right.inorder_traversal();
        }
    }

    pub fn preorder_traversal(&self) {

        println!("{}", &self.val);

        if let Some(left) = &self.left {
            left.preorder_traversal();
        }

        if let Some(right) = &self.right {
            right.preorder_traversal();
        }
    }
}

pub fn rebuild_tree(inord_arr: &Vec<i32>, preord_arr: &Vec<i32>) -> Node {
        
    let root_val = preord_arr[0];
    let mut root = Node::new(root_val);

    // Find the index of root from inorder result
    let inord_arr_root_i = inord_arr
        .iter()
        .position(|&x| x == root_val)
        .unwrap();

    let inord_left_sub_len = inord_arr_root_i;

    if inord_left_sub_len == 1 {
        root.left = Some(Box::new(Node::new(inord_arr[0])));
    } else if inord_left_sub_len > 1 {
        let preord_left_sub_vec = preord_arr[1..inord_left_sub_len+1].to_vec();
        let inord_left_sub_vec = inord_arr[..inord_arr_root_i].to_vec();
        root.left = Some(
            Box::new(rebuild_tree(
                &inord_left_sub_vec,
                &preord_left_sub_vec
            ))
        );
    }

    let inord_right_sub_len = inord_arr.len() - inord_left_sub_len - 1;
    if inord_right_sub_len == 1 {
        root.right = Some(Box::new(Node::new(inord_arr[inord_arr_root_i + 1])));
    } else if inord_right_sub_len > 1 {
        let preord_right_sub_vec = preord_arr[1+inord_left_sub_len..].to_vec();
        let inord_right_sub_vec = inord_arr[inord_arr_root_i+1..].to_vec();
        root.right = Some(
            Box::new(rebuild_tree(
                &inord_right_sub_vec,
                &preord_right_sub_vec
            ))
        );
    }

    root
}

fn main() {
    let inord_vec = vec![1,2,3,4,5,6,7];
    let preord_vec = vec![5,2,1,4,3,6,7];
    let rebuild_tree_n = rebuild_tree(&inord_vec, &preord_vec);

    rebuild_tree_n.inorder_traversal(); // 1,2,3,4,5,6,7
    println!("--------");
    rebuild_tree_n.preorder_traversal(); // 5,2,1,4,3,6,7
}
```