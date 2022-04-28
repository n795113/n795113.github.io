---
title: Learn Solana CLI
date: 2022-04-22 08:19:39
categories: [ Crypto ]
tags:
  - solana
  - crypto
---

### Wallet types
- file
- paper
- hardware

### Create wallet

create wallet (key-pair)

```shell
// file wallet
solana-keygen new --outfile YOUR-PRIVATE-KEY-FILE-PATH
// paper wallet
solana-keygen new --no-outfile
```

the public key will display on the terminal
if you didn't save the pubkey, it's fine.
we can get the pubkey from private-key

```shell
solana-keygen pubkey YOUR-PRIVATE-KEY-FILE-PATH
```

Verify key-pair

```shell
solana-keygen verify PUBKEY PRIVATE-KEY-FILE-PATH
```

### Interact with wallet

- `pubkey` aka. `address` aka. `wallet address`

config
```
solana config get
```

set URL to dev-net for developing
```
solana config set --url https://api.devnet.solana.com
```

send 1 SOL to address
```
solana airdrop 1 PUBKEY
```

get balance of address
```
solana balance PUBKEY
```

transfer

- from
- to
- amount
- fee-payer

```
solana transfer --from <KEYPAIR> \
<RECIPIENT_ACCOUNT_ADDRESS> <AMOUNT> \
--fee-payer <KEYPAIR>
```

## Stake 

### create stake account
```
// first create a keypair for account address
solana-keygen new --no-passphrase -o stake-account.json

// then create account
solana create-stake-account --from <KEYPAIR> stake-account.json <AMOUNT> \
    --stake-authority <KEYPAIR> --withdraw-authority <KEYPAIR> \
    --fee-payer <KEYPAIR>
```

### get account
```
solana stake-account <STAKE_ACCOUNT_ADDRESS>
```

### update account auth
```
solana stake-authorize <STAKE_ACCOUNT_ADDRESS> \
    --stake-authority <KEYPAIR> --new-stake-authority <PUBKEY> \
    --fee-payer <KEYPAIR>
```

## Delegate Stake (質押)

### list available validators
```
solana validators
```
### delegate
```
solana delegate-stake --stake-authority <KEYPAIR> <STAKE_ACCOUNT_ADDRESS> <VOTE_ACCOUNT_ADDRESS> \
    --fee-payer <KEYPAIR>
```

### deactive
```
solana deactivate-stake --stake-authority <KEYPAIR> <STAKE_ACCOUNT_ADDRESS> \
    --fee-payer <KEYPAIR>
```

### withdraw sol
only work when the stake-account deactived
```
solana withdraw-stake --withdraw-authority <KEYPAIR> <STAKE_ACCOUNT_ADDRESS> <RECIPIENT_ADDRESS> <AMOUNT> \
    --fee-payer <KEYPAIR>
```
### split 
You may want to delegate stake to additional validators while your existing stake is not eligible for withdrawal. It might not be eligible because it is currently staked, cooling down, or locked up. To transfer tokens from an existing stake account to a new one
```
solana split-stake --stake-authority <KEYPAIR> <STAKE_ACCOUNT_ADDRESS> <NEW_STAKE_ACCOUNT_KEYPAIR> <AMOUNT> \
    --fee-payer <KEYPAIR>
```
    
## Reference
- https://docs.solana.com/cli/usage