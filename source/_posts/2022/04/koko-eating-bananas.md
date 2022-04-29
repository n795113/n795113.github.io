---
title: "Solve Leetcode in Rust: Koko Eating Bananas"
date: 2022-04-30 00:05:19
categories: [Tech, Rust]
tags:
  - leetcode
  - rust
  - bst
---

## Problem
Problem: [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

Let's draw some pictures to understand this quesiton. If there are 4 piles of bananas like below, the given array would be `[3, 2, 3, 4]`.

![](https://i.imgur.com/qLpwHkN.png)

Then if Koko can eat 3 bananas per hour, it will takes her 5 hours to eat all the bananas like blow.

![](https://i.imgur.com/WTtMmez.png)

Now, if the limit hours `h` is 5 then 3 bananas per hour satisfies. However, if `h`, for example, is 3, then Koko need to SPEEDS UP to finish all the bananas. On the other hand, if `h` is 10 which means we still have time right? So Koko can SLOW DOWN to certain speed to make herself chill but still can finish all the banan in time.

![](https://i.imgur.com/2QpNhfI.png)

So what is the best eating speed is the question!

## Thoughts

- The maximum of speed should equals to the number of the bananas of the tallest pile
- The minimun of speed should equals to 1

Which means we are trying to find the best from `1` ~ `piles.max()`. Searching a value from a sorted array... maybe binary search is a soslution.

## Code

### Try #1
```rust
use std::cmp::Ordering;

impl Solution {
    pub fn min_eating_speed(piles: Vec<i32>, h: i32) -> i32 {
        let mut sorted_piles = piles.clone();
        sorted_piles.sort();
        Solution::min_eating_speed_inner(
            &sorted_piles,
            h,
            1,
            *sorted_piles.last().unwrap()
        )
    }

    fn min_eating_speed_inner(piles: &Vec<i32>, h: i32, min: i32, max: i32) -> i32 {
        // return codition
        if piles.len() == 0 || min > max {
            return -1;
        }

        // speeds arr is [min..max]
        // so the mid speed is (min + max) / 2
        let mid = min + (max - min) / 2;

        // piles divides speed return spend time
        let time = Solution::count_eating_time(&piles, mid);

        // find the matched speed or continue go down to sub tree
        match time.cmp(&h) {
            // spend too much time to eat, speed up
            Ordering::Greater => Solution::min_eating_speed_inner(piles, h, mid + 1, max),
            // match, try to find a better (slower) speed
            _ => {
                let new_speed = Solution::min_eating_speed_inner(piles, h, min, mid - 1);

                if new_speed > 0 {
                    return new_speed;
                }
                mid
            }
        }
    }

    fn count_eating_time(piles: &Vec<i32>, speed: i32) -> i32 {
        // count the time to eat each pile and return sum
        let sum = piles
            .into_iter()
            .map(|pile| {
                if pile % speed > 0 {
                    return pile / speed + 1
                }
                return pile / speed
            })
            .sum();

        sum
    }
}
```
