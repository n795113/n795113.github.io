---
title: Burn of BNB
date: 2022-03-17 12:48:03
categories: crypto
tags:
    - crypto
    - bnb
---
## About burn

### When

- burn as gas fee ([BEP-95](https://academy.binance.com/en/glossary/bep-95))
- burn by quarter
- burn by price ([Auto-Burn](###Auto-Burn))
- some smart contracts also burn BNB in some conditions

### Why

To make deflation

### How

Send tokens to an address without a private key, which means no one can access the tokens anymore.

## BEP-95

### Why

To fasten the burn efficiency.

### How

Since the solution is to burn partial of validators’ reward, the more users use BSC (which means more blocks need to be validated by validators), the more BNB would be burned.

## Auto-Burn

### Rule

The more the price of BNB decreases, the more BNB will be burned.

When the total supply is lower than 1B, the burn process stops (BEP-95 still works).

### Formula


```
B = N * 1000 / (P + K)
```

- B: how many BNB to burn
- N: new BNB amount of this quarter
- P: average price
- K: constant value, 1000
    
More detail about [Auto-Burn](https://www.binance.com/en/blog/ecosystem/introducing-bnb-autoburn-a-new-protocol-for-the-quarterly-bnb-burn-421499824684903205)