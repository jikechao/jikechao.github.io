---
layout:     post
title:      "Block Chain"
subtitle:   " \"Block Chain, ETH\""
date:       2019-11-24 22：04
author:     "Jack-C"
header-img: "img/post-bg-blockChain.jpeg"
catalog: true
tags:
    - BlockChain
    - 学习
---



### 1、ETH概述

- 以太坊：一种重要的加密货币，对BTC出现的问题进行改进，是区块链2.0版本
- BTC与ETH区别：
  - ETH改进了BTC的出块时间
  - ETH是memory hard，用于防止ASIC矿机挖矿，是ASIC resistance，
  - 将proof of work，改成 proof of stack
  - ETH增加对智能合约smart contract 的支持

### 2、ETH-账户

- ETH优点
  - ETH是account-based ledger,有余额的概念，不需要说明币的来源
  - ETH具有天然的防double spending attack优势，花一次扣一次钱
- ETH缺点：
  - replay attack：收款方是个坏蛋，故意将转账记录重放一次，扣转账方两次钱
    - 解决方案：同时维护balance&交易的次数nonce,每次转账时，将transaction+nonce一起签名，并发布。若当前nonce=20，下次交易若nonce不是21，则不理他。
- 账户：
  - 外部账户（externally owned account）：包含balance 、nonce
  - 合约账户（smart contract account）：
    - 不能主动发起交易，可以被外部账户调用，同时也可以调用其他合约账户；
    - 包含code和storage
    - 创建合约将返回一个地址，知道这个合约的地址，将可以调用这个合约

