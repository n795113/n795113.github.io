---
title: 超白話理解 Solana 代幣與 NFT
date: 2022-04-22 22:22:39
categories: [Crypto]
tags:
  - solana
---
## 前言

希望能以最直觀的方式解釋 Solana 鏈上的代幣，也就是 Token。

注意！這邊特指是 Solana 的，因為會跟乙太坊的代幣細節上有諸多不同，有興趣可以再去研究一下兩者的差別。

本文適合那些看了[官方文件](https://spl.solana.com/token)還是霧嗄嗄 (bū-sà-sà) 的人，像我自己。


## 銀行帳戶
<!-- 解釋一種代幣對應一種帳戶的概念 -->

以外匯為例，假設阿蟹今天要在銀行存美金、日幣、和歐元，那他勢必要開三個對應的帳戶。美金帳戶只能存美金、日幣帳戶只能存日幣、歐元帳戶只能存歐元。

而 Solana 的代幣其實也是一樣的規則，當我們想要持有某一代幣 (token) 時，我們就必須先創建那個代幣的帳戶 (associated account)，然後才能把代幣轉過去。

所以想要持有幾種代幣，就需要幾個代幣帳戶。

## 鑄幣權
<!-- 只有代幣創始者可以鑄造 -->
而除了持有各種幣，我們也可以在 Solana 上發行自己的代幣。

以美金做比喻的話，「發行」就是美國央行跟大家宣布他要發行一種貨幣，代號為 USD；在實際鑄造 USD 前，USD 的總發行量會為 0。

接著美國央行可以決定要鑄造多少 USD，而其他人都不可以鑄造 USD。美國央行鑄造出來的 USD ，可以指定給任意對象：可以給央行自己的 USD 帳戶，也可以給到任意對象的 USD 帳戶。

所以鑄幣權的概念其實滿直觀的：只有該代幣「發行人」有權鑄造 (mint) 該代幣，其他人無法鑄造，但可以被指定爲鑄造出來的代幣的「接收者」 (recipient)，前提是要擁有該代幣的帳戶。


## NFT
<!-- 跟一般代幣的差別 -->

其實在 Solana 鏈上，NFT 跟我們一開始談的代幣本質上沒有區別，都是 token。

在 Solana 上發行代幣時，我們可以設定最小單位，預設為 0.000000001；而總量可以有上限，也可以無限。而 NFT 只是最小單位為 1，且發行總量為 1 的代幣！

NFT on Solana：
- 最小單位為 1
- 鑄造 1 後即關閉鑄造

而且一樣的，要存一個 NFT，我們也需要開一個對應的帳戶去存他。

## 實際操作

到上面，其實就已經把 Solana 代幣的概念講完了。

沒錯，就是這麼簡單～

以下是比較技術的部分。我們會實際操作 CLI 實作代幣的發行。

英文不錯的可以直接照著[官方文件](https://spl.solana.com/token)操作。

### 發行代幣
```
$ spl-token create-token
```
執行結果：
```
$ spl-token create-token
Creating token 4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd

Signature: 55e32Jz8qdNxsBHd6TKvGQnuPHvDdKf2vYRU8f1CM4UWNR3FaFtYzmMYCHdzJ15z5CZpeXKu4Qgtcf2Bph136KK82qYQ6JPguUvtktoe6y43BAvgN2C3iyYHWZX1GAB6TbXD3B1nNRHFk4Rxcx7uhnAcB4SUGTXTG4
```
這樣就發行代幣了！而該代幣的代號為：`4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd`。每次代號都不同，所以你自己實作時會產生你自己代幣的代號。以下我們都用 `<代幣 ID>` 表示。

### 鑄造

發行完後，我們可以查看該代幣總量
```
$ spl-token supply <代幣 ID>
```
執行結果：
```
$ spl-token supply 4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd
0
```
目前代幣總量應為 0，因為我們還沒鑄造任何代幣。

### 代幣帳戶

開一個代幣帳戶，此帳戶「只能」存該代幣。
```
$ spl-token create-account <代幣 ID>
```
執行結果：
```
$ spl-token create-account 4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd
Creating account 9Vz65o3NNNJvhPnheCr6MfFx7HUwxnZt4JhLSXEmmfqz

Signature: smvyxxvZmktfex1dT5umcbqAYZecc2ZFesNUAFQhGfbbLkj2NHM5QJcCZHKrciAVa8gBecMbTFVWhL3k23qSRpM
```
所以我們的代幣帳號為 `9Vz65o3NNNJvhPnheCr6MfFx7HUwxnZt4JhLSXEmmfqz`，他是專門用來存代幣 `4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd` 的。

### 鑄造

```
$ spl-token mint <代幣 ID> <欲鑄造數量> <接收的代幣帳戶>
```
執行結果：
```
$ spl-token mint 4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd 100
Minting 100 tokens
  Token: 4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd
  Recipient: 9Vz65o3NNNJvhPnheCr6MfFx7HUwxnZt4JhLSXEmmfqz

Signature: SY6xk2KQNrvizeaCEMvbbQKgPGUYKPZWusjdwWtoAbauqLB8Y3qdKhE6Hp46yhut7z2qcwqSi67uPf6DzA3FfRD
```
如果 `<接收的代幣帳戶>` 沒給定，預設會是自己的代幣帳戶；如果之前沒有創建代幣帳戶，這次的鑄造就會失敗。

我們成功的轉了 100 的 `4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd` 代幣到我 `9Vz65o3NNNJvhPnheCr6MfFx7HUwxnZt4JhLSXEmmfqz` 的帳戶了。

查看我們的帳戶：
```
$ spl-token accounts
```
執行結果：
```
$ spl-token accounts
Token                                         Balance
---------------------------------------------------------------
2KN5U3jFNC5hf5tCELzg86zi27urr81XMo5MGkC4PiKY  0.100000001
2XkLQPjaXDsRSyF5xV8TBArBwwmUsVmviAyMx675EFDt  1
3M41XViQ3ARAgwEd3Nq3xZEtE7fZEQQd4X25gumMPLQQ  22
4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd  100
6Ta6A8XaohhU1pAaRSKkmuMAKJyVLuYAXizKSyNyaueK  2
95dgoH52qHRapAb8DtW4uQxdcMiyhqG4cS1J4UH8744G  0.11
CmyPi9HBo1yszknenu71vWJaJ9VT5Lnt5Q7SpBXQK4AY  1
GZeBB6EdP4Yrdx9jHed15dCj9ToA9ff8uQiTkfuW6Xc9  1
```
可以看到第 4 行是我們剛剛新發行的代幣，有 100 枚，沒錯。其他是我所持有的其他代幣，與與其對應的持有數量。

輸入以下指令可以看得更清楚我們是在「哪個」帳戶持有「什麼」代幣：
```
$ spl-token account -v
```
執行結果：
```
$ spl-token account -v
Token                                         Account                                       Balance
----------------------------------------------------------------------------------------------------------
2KN5U3jFNC5hf5tCELzg86zi27urr81XMo5MGkC4PiKY  14si8Shrx9vnVMBbFq5QEgjX1iwG4d7Ffmt6zoHdcA1Q  0.100000001
2XkLQPjaXDsRSyF5xV8TBArBwwmUsVmviAyMx675EFDt  2o6ud26e1bCPYdfMjS1ctHYXXXYPRaEMxLq3WFmC8jyv  1
3M41XViQ3ARAgwEd3Nq3xZEtE7fZEQQd4X25gumMPLQQ  CZ7645yM6g5JFcvHPBVXN8xWKdRdBiykXNLdF1t2kbbe  22
4o4wYDr7mQGFFawdhdaHCz5CTTeZtsdw4aVdyCApXZHd  9Vz65o3NNNJvhPnheCr6MfFx7HUwxnZt4JhLSXEmmfqz  100
6Ta6A8XaohhU1pAaRSKkmuMAKJyVLuYAXizKSyNyaueK  ypBba7nbYHgNg6T6L37iK2C52DAMJm8vB4Q8Qx66m8y   2
95dgoH52qHRapAb8DtW4uQxdcMiyhqG4cS1J4UH8744G  7xonGX7bnV31XL7BDX1YJSEQUBSrd5mb3FdNyLbkEaNZ  0.11
CmyPi9HBo1yszknenu71vWJaJ9VT5Lnt5Q7SpBXQK4AY  HVo4VECQ14Ffq5922E5X5yVbwsSebSESatt6UviZLFtA  1
GZeBB6EdP4Yrdx9jHed15dCj9ToA9ff8uQiTkfuW6Xc9  8uyD4nnnqRmV1mVuLE6hmfAiqVt7Krvc7tAmat2NN5cx  1
```

### 發行 NFT

在觀念說明時說過，NFT 在 Solana 跟一般的 token 其實沒有本質上的區別，只是最小單位、與總發行量都為 1。實作起來會如下：

`-- decimals 0` 是小數點後有 0 位的意思，所以該代幣只能持有整數，如果嘗試轉 0.1 的話不會噴錯，但帳戶數量不會增加，有興趣可以試試。
```
$ spl-token create-token --decimals 0
```
執行結果：
```
$ spl-token create-token --decimals 0
Creating token 9J7Bmg8Yx4dPsSiiQAvdHuwLS5dCf9ET6jyamwpaJUKR

Signature: 5aHJuH1eBvzHQZ2NCHPVPcCioKyfJdiPkhtTVtbfCvUcqeUrZ1vohf1i59pcmZWz8jo9pcqJ9ZkBvAxXstJStwGY
```
接著一樣是要創建對應的帳戶然後鑄造：
```
$ spl-token create-account 9J7Bmg8Yx4dPsSiiQAvdHuwLS5dCf9ET6jyamwpaJUKR

$ spl-token mint 9J7Bmg8Yx4dPsSiiQAvdHuwLS5dCf9ET6jyamwpaJUKR 1
```
注意，這裏其實你也可以 mint 1 個以上，也可以成功，但這樣這個 token 總量就會有 1 個以上，就不是所謂的 NFT 了。

成功 mint 1 個代幣後，我們馬上關閉該代幣的鑄造功能。

這個關閉事不能再打開的，如此就達成意義上的 NFT 了，每個 token 都是獨一無二。

命令：
```
$ spl-token authorize <代幣 ID> mint --disable
```
執行結果：
```
$ spl-token authorize 9J7Bmg8Yx4dPsSiiQAvdHuwLS5dCf9ET6jyamwpaJUKR mint --disable
Updating 9J7Bmg8Yx4dPsSiiQAvdHuwLS5dCf9ET6jyamwpaJUKR
  Current mint authority: 46hytJBhguswo6S8fCcVtR85HEnb9nd1hwMxFWYnSHXc
  New mint authority: disabled

Signature: 295NVTrVZYSygVXZbNwG91HE99GaosN2QA6U4fNZ5v976nFQXWRk1suLNqM81smniTuH92QgG6YBLfHQ8eM6bEwW
```

## 總結

- 如果想要持有 1 種代幣，必須擁有 1 個對應代幣的帳戶
- NFT 是一種最小單位與發行量為 1 的代幣


## Reference
- https://spl.solana.com/token
- https://pencilflip.medium.com/solanas-token-program-explained-de0ddce29714