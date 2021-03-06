---
layout:     post
title:      "Block Chain"
subtitle:   " \"Block Chain, BitCoin\""
date:       2019-11-24 20：04
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - BlockChain
    - 学习
---





## 1、BTC-密码学原理 

1. 比特币：
   1. 一种加密货币（实际上并没有加密）
   2. 密码学原理：哈希&签名
   3. 先对一个message取hash，然后对hash签名
   4. 使用哈希函数：SHA-256（Secure Hash Algorithm-256）
2. 相关原理：**x = Hash(x)**
3. 没有任何函数可以数学上证明是collision resistance的，只能通过实践经验
4. 对于给定的H(x)，仅能通过暴力枚举，当输入空间足够大时，便成为x为hiding的
5. digital commitment = digital equivalent of a sealed envelope：
   - 解释：对于一个结果x，首先计算H(x)并将其公布出去，在找到某个x', H(x')==H(x)**之后**,验证x' ==x，并且人们无法篡改x。
   - 注：输入空间一定要足够大，且分布均匀
6. 性质：
   - collision resistance ：
   - hiding：不能通过轻易地通过H(x) 值找到x
   - puzzle friendly ：
7. 挖矿的本质：
   - 寻找随机数nonce（只能暴力，是一种工作量证明）
   - 使得H(block header)  <= target
   - 原理：difficult  to solve（工作量证明），but easy to verify（一次哈希验证）
8. BTC开户方法：
   1. 在本地创建一个公私钥对（public key, private key），开户成功，不需要中心机构
   2. 公钥相当于银行卡账号，私钥相当于账户密码
   3. 用私钥签名，要公钥验证签名
   4. 需要一个好的随机源，使得不可能产生两个相同的公钥

## 2、BTC-数据结构

1. 概念：比特币的数据结构就是区块链，区块链就是一个个区块组成的链表
2. 区块链和普通链表区别：
   - 用hash指针代替普通指针
   -  ![1574425144741](https://github.com/jikechao/img-folder/blob/master/1574425144741.png?raw=true)
   - 对于后一个块的哈希值，是通过前一个区块的**整个**的内容（包括hash point）取哈希
   - 通过这样的数据结构可以实现：temper-evident log ：只需要记住最后一个块的哈希值，便可以检测区块链中任何部位的修改。改变前面任何一个区块，便会影响后面所有的区块信息。
3. Merkle tree和binary tree的区别
   - Merkle tree的指针代替了普通指针
   - ![1574426537907](https://github.com/jikechao/img-folder/blob/master/1574426537907.png?raw=true)
   - 只需要技术root hash 就可以检测出所有部分的修改，保护整棵树，比上面那个图还高效。
   - 每个data block 是一个transaction，，每个块分为块头block header和块身block body
   - block header用于存放本区块所有信息的哈希值，不包含交易的具体内容
   - block body中存放交易列表
   - Merkel proof：找到交易所在的节点，依次向上找到到根结点的一条路径，便是Merkel proof
4. 轻结点和全节点：
   - 轻结点：比如手机上的BTC钱包，不保存交易列表，只保存根哈希值
   - 全节点：保存整个区块链信息，目前大于200G
   - spv支付验证：轻结点向全节点，请求当前交易是不是包含在Merkel tree里面的Merkel proof ，全节点收到请求后，将路径上的哈希值发给轻结点，轻结点在本地可以计算出当前交易所在的哈希值,直到计算到根节点的哈希值并验证，若验证成功，说明，当前交易支付成功。
   - ![merkel_proof](https://github.com/jikechao/img-folder/blob/master/Merkel_proof%20.png?raw=true)

## 3、BTC 实现

1. 记账方式：
   1. BTC：transaction-based ledge（账单）：
   2. 以太坊：account-based Ledger：系统显示记录余额
2. UTXO：Unspent Transaction Output，
   - **全节点**在**内存**中维护UTXO
   - 用于检测账户余额,防止double spending
   - total inputs ==total outputs（outputs 可能略小于inputs，用于缴纳交易费transaction fee）
   - 挖矿：
     - 主要争夺记账权，获取出块奖励，目前是12.5BTC
     - 每隔四年出块奖励减半
     - 每次挖矿的过程看成一次伯努利trial：a random experiment with binary outcome
     - 伯努利trial是memoryless，下次一实现和之前的所有实验结果均无关
     - 若挖了10min还没出块，估计还要再挖10min，虽然平均出块时间是10min。（类比掷硬币）保证挖矿公平性。
3. BTC数量：
   - 出块奖励是产生新的BTC的唯一途径，通过CoinBase 每隔十分钟**左右**产生一个block reward，block reward每隔约4年减半，目前是12.5BTC
   - BTC总量=21万区块\*50出块奖励+21万\*25+21万\*12.5+……=2100万
   - 通过挖矿来维护系统的安全性，只要大部分算力掌握在诚实人手里就行。挖矿就是比拼算力啊，没啥难度滴
4. BTC安全性分析
   - 由于BTC是只认最长合法链，对于获得记账权的坏蛋，想要写一个非法的交易进入区块中，由于缺少币主人的私钥，无法签名，导致诚实人不认账，好人奖继续从前一个区块挖矿，这样他将有可能损失一个block reward。
   - double spending attack问题：
   - ![1574482574747](https://github.com/jikechao/img-folder/blob/master/1574482574747.png?raw=true)
     - 攻击方式：M现将前转给A，再将前转给他自己M',并将下面的区块扩展成为最长合法链
     - 解决方案：经过6个confirmation才认为前面的交易是不可篡改的。约1个小时等待时间，才认为交易确实不可篡改。 
     - 实际上很多zero confirmation：
       - 诚实的矿工只认可第一次发布的交易
       - 购物网站交货天然存在交易时间，若发现交易没有发布在最长合法链上可以终止发货。
   - selfish mining：
     1. 用于分叉攻击，等违法的东西6个confirmation，一下发布7个区块，成为最长合法链，但是由于算力原因，不大可能藏了很多块。
     2. 挖出来区块先藏着，自己接着偷偷的继续挖，等别人挖出下一个区块之前，赶紧发布，有风险滴。

## 4、BTC-网络

1. ![1574486552184](https://github.com/jikechao/img-folder/blob/master/1574486552184.png?raw=true)
2. 每个区块大小：不大于1M



## 5、BTC-挖矿难度

1. $$
   H(block header) <= target
   $$

   target为目标阈值，target越小，挖矿难度越大，调整挖矿难度就是调整目标空间在所有空间中所占的比例，挖矿难度和目标阈值成反比

2. BTC所有的哈希算法为SHA-256,

   1. 输出空间为 $2^{256} $,

3. 挖矿难度（最小是1，意识阈值非常大）

   - difficult = difficult_target_1/target       //difficult_target_1：挖矿难度==1时，所对应的目标阈值

4. 调整挖矿难度的原理：

   - 系统中总算力越来越强，出块时间会越来越短
   - 出块时间10min是人为设定的，要保持稳定，需要调整挖矿难度

5. 出块时间设定：

   - 出块时间若非常快，区块链会经常分叉，若出现多分叉，系统中的总算力会被分散，则51%一下的算力就可以发动攻击

6. 调增挖矿难度方法

   1. 2016个区块调整一次挖矿难度：约为14天

   2. **调整公式**：
      $$
      target = target * actual\ time/expected\ time
      $$

   3. target的调整是写在BTC代码中的，代码开源的，若坏蛋就不调整target（存在nbits中）则不能通过诚实的矿工合法性验证

## 6、BTC-挖矿

1. 全节点：
   - 一直在线
   - 在本地硬盘上维护完整的区块链信息
   - 在内存里维护UTXO集合，用于快速检验交易的正确性
   - 监听BTC网络上的交易信息，验证每个交易的合法性
   - 决定哪些transaction被打包到区块里
   - 监听别的矿工挖出来的区块，验证合法性
   - 挖矿
     - 决定沿着那条链挖下去
     - 出现等长的分叉，选择哪一个分叉
2. 轻节点：
   - 不是一直在线
   - 只保存block header
   - 不用保存全部transaction，只保存自己相关的
   - 无法检测大多数交易的合法性，检验与自己相关的transaction的合法性
   - 无法检测网上发布的区块的正确性
   - 可验证挖矿难度
   - 只能检测哪个是最长链，不知道哪个是最长合法链
3. BTC安全性保证：
   1. 密码学保证：签名
   2. 大多数矿工都是好人
4. 挖矿设备：
   - CPU：通用计算
   - ->GPU：通用**并行**计算，但很多浮点运行还是闲置
   - ->ASIC芯片(ASIC矿机)：application specific Integrated Circuit   ：只针对具体的mining puzzle，只能计算哈希值，只针对某一单一加密货币挖矿
5. 挖矿人员：
   - 个人矿工：全节点所有任务
   - 矿池：pool manager管理很多miner，可以解决收入不稳定问题
     - 矿主：全节点的其他功能
     - 矿工：计算哈希值
   - 大型矿池，若掌握51%算力，可以发动攻击，矿工还蒙在鼓里
     - 分叉攻击
     - Boycott：就是不把某个账户的transaction写进区块中，公开抵制账户，但凡有人打包transaction，他就分叉

## 7、BTC-脚本语言

## 8、BTC-分叉

1. 分叉原因

   - 两个矿工同时挖到矿：state for
   - 分叉攻击：forking attack：deliberate for
   - 升级软件，导致用不同版本的协议：protocol fork
     - 根据协议修改内容不同：
       - 硬分叉
       - 软分叉

2. 硬分叉:hard fork

   - 特点：必须系统中所有节点都更新，才不会出现分叉，但凡有不愿意更新的，会出现永久分叉

   - block size limit，由于每个区块仅1M大小，假设通过硬分叉1M->4M，
   - 结果：新节点认可旧节点，旧节点不认可新节点。最终造成分家，不认可协议，但认可transaction，但会通过回放，将新节点上的transaction，在旧节点上回放一下。
   - 真实案例：ETH，ETC
   - 区块并不是越大越好：BTC底层网络采用P2P，大区块很消耗贷款

3. 软分叉：soft fork,

   - 特点：会出现临时分叉，但是不会出现永久分叉
   - 如：1M-> 0.5M
   - 后果：新节点挖小区块，旧区块挖大区块，旧节点认为合法的交易，新节点可能不认可，新节点认为合法的，旧节点一定认可。
   - 实际案例：coinbase域：前八个字节可以作为extra nonce 增加搜索空间，剩下的域还没定义：
     - 有人提议：将UTXO集合内容组成Merkel tree，算出根哈希值，写在coinbase域中
   - 著名案例：P2SH：pay to Script Hash

## 9、BTC-问答

1. BTC私钥丢失，怎么办？

   答：钱就永远取不出来

2. BTC私钥泄露？

   答：吧剩余钱转到安全账户上。

3. 钱转错了地址？

   答：没有办法，认栽。若转到不存在的账户，就变成了死钱了。
