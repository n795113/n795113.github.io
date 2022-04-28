---
title: "Solve Leetcode in Rust: 0034 Find The First And The Last Position of Element In A Sorted Array"
date: 2022-04-28 11:23:07
categories: [Tech, Rust]
tags:
  - rust
  - leetcode
  - bst
---
## Thoughts

The core concept is to find the target from a sorted array which is quite suitable to use binary serach I think.

Here are my instructions:

1. Get the middle element, `mid`
2. Compare the `mid` with the target. there will be 3 branches:
    a. `target` < `mid`: continue to find the target from the LEFT side of the `mid`
    b. `target` > `mid`: continue to find the target from the RIGHT side of the `mid`
    c. `target` = `mid`: In this case, `[index_of_mid, index_of_mid]` will be our base answer so keep it. Then based on this answer, we need to go further to the both left and right to seek if there is other elements match to the search target.

For example, if the given array is `[1, 2, 3, 4, 4, 4, 4, 6, 9]`, and the search target is `4`.

Then we are trying to get this green range.

![](green_range.png)

When we go through the instructions, we will get the middle element, and hit the c case.

![](mid.png)

Then base on this position, we seek towar left and right to get the final range.

![](seek_left_n_right.png)

So the answer should be `[3, 6]`

## Code

### Try #1
```rust
impl Solution {
    pub fn search_range(nums: Vec<i32>, target: i32) -> Vec<i32> {
        if nums.len() == 0 {return vec![-1,-1]}
        bst_search_index_rec(&nums, target, 0, (nums.len()-1) as i32)
    }  
}

fn bst_search_index_rec(arr: &Vec<i32>, val: i32, start_i: i32, end_i: i32) -> Vec<i32> {
    if start_i > end_i {
        return vec![-1, -1];
    }

    let mut result: Vec<i32> = vec![-1, -1];

    let mid_i = (start_i + end_i)/2;

    if val == arr[mid_i as usize] {
        result[0] = mid_i as i32;
        result[1] = mid_i as i32;
        
        let tmp_left_result = bst_search_index_rec(&arr, val, start_i, (mid_i-1) as i32);
        if tmp_left_result[0] != -1 {
            result[0] = tmp_left_result[0];
        }

        let tmp_right_result = bst_search_index_rec(&arr, val, (mid_i+1) as i32, end_i);
        if tmp_right_result[1] != -1 {
            result[1] = tmp_right_result[1];
        }

    } else if val < arr[mid_i as usize] {
        result = bst_search_index_rec(&arr, val, start_i, (mid_i - 1) as i32);
    } else if val > arr[mid_i as usize] {
        result = bst_search_index_rec(&arr, val, (mid_i + 1) as i32, end_i);
    }

    return result;
}
```

### Try #2
```rust
use std::cmp::Ordering; // Don't forget to import this

impl Solution {
    pub fn search_range(nums: Vec<i32>, target: i32) -> Vec<i32> {

        Solution::bst_search_index_inner(&nums, target, 0, nums.len() as i32 - 1)
    }
    
    fn bst_search_index_inner(arr: &Vec<i32>, val: i32, start_i: i32, end_i: i32) -> Vec<i32> {
        // return condition
        if start_i > end_i || arr.len() == 0 {
            return vec![-1, -1];
        }

        // avoid overflows rather than simply (start_i + end_i) / 2
        let mid_i = start_i + (end_i - start_i) / 2;

        match val.cmp(&arr[mid_i as usize]) {
            Ordering::Less => Solution::bst_search_index_inner(&arr, val, start_i, mid_i - 1),
            Ordering::Greater => Solution::bst_search_index_inner(&arr, val, mid_i + 1, end_i),
            Ordering::Equal => {
                let l = Solution::bst_search_index_inner(arr, val, start_i, mid_i - 1);
                let r = Solution::bst_search_index_inner(arr, val, mid_i + 1, end_i);

                match (l[0], r[1]) {
                    (-1, -1) => vec![mid_i, mid_i],
                    (-1, new_r) => vec![mid_i, new_r],
                    (new_l, -1) => vec![new_l, mid_i],
                    (new_l, new_r) => vec![new_l, new_r]
                }
            }
        }
    }
    
}
```

## Conclusions
### Improvements

- avoid overflows in some edge cases
- move recursion function into `Solution` struct
- more rusty coding style

### Notes:
- Combo of `cmp` & `match`
- How to use the function itself in the implementation -> `Solution::bst_search_index_inner`