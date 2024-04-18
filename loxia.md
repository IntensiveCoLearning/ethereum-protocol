## loxia

哈喽大家，我是 loxia.eth，来这里补课学习一些基础且重要的知识，很高兴能来到这里和大家一起学习。

## Notes

### 2024.4.18

Slot 和 POS 交易排序详解：https://www.paradigm.xyz/2023/04/mev-boost-ethereum-consensus

如果 Validators 在 t = 4 的证明截止时刻结束前没有看到新的块，将投票给先前的链头，区块提出的越早，传播的时间就越多，因此积累证明就越多。从网络健康的角度来看，发布区块的最佳时间是 t=0（根据规范规定），然而，由于区块的价值随着时间单调增加，提议者有动机推迟区块的发布，以积累更多的 MEV，细节详见：https://ethresear.ch/t/timing-games-in-proof-of-stake/13980

从历史上看，只要下一个 validator 在为下一个 slot 构建块之前观察到该块，提议者就可以在证明截止时刻之后很久（甚至接近 slot 结束时）发布一个块。这是子块继承父块的权重以及分叉选择规则终止于叶节点的结果。因此，推迟区块发布并没有什么坏处。为了帮助推动理性行为（延迟区块发布）转向诚实行为（按时发布），实施了“诚实重组”（“honest reorgs”）。

共识客户端引入了两个新概念，这对证明截止日期具有重要影响：proposer boost (https://github.com/ethereum/consensus-specs/pull/2730) ；honest reorgs (https://github.com/ethereum/consensus-specs/pull/3034) 

proposer boost：尝试通过授予提议者相当于完整证明权重 40% 的分叉选择“boost”来最大程度地减少 balancing-attacks（https://arxiv.org/abs/2009.04987）攻击。重要的是，这种 boost 仅持续该 slot 的持续时间（12s）。

honest reorgs：接受 proposer boost，并允许诚实的提议者使用它来强制重组证明权重低于 20% 的区块。（非强制更改）

在这些特殊情况下会避免 honest reorgs：during epoch boundary blocks; if the chain is not finalizing; if the head of the chain is not from the slot prior to the reorged block

条件 3 确保 honest reorgs 仅从链中删除单个块，这充当断路器，允许链在极端网络延迟期间继续生成块。这也反映了 proposer 对其网络观点的信心下降，因为他们不再确定他们的 proposer boost 的区块将被视为规范。

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95400362/f534a2b7-bed3-4734-bd54-1ecddef2ba6c)



### 2024.4.17

Week 3 CL：https://epf.wiki/#/eps/week3

前置学习：https://ethereum.org/zh/developers/docs/consensus-mechanisms/pos
阅读 Chloe 笔记：https://ab9jvcjkej.feishu.cn/docx/X7Ard9UlPoj2lmxKMvucfNubnTb

BFT 拜占庭容错问题，BTC 的 POW 解决方案最优

每12秒一个 Slot，每个 Slot 都会有一个区块。
![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95400362/0cb7e69a-e4d4-41c0-bffb-80d52aa5c4d1)

每个 Epoch 有 32 个 Slot，设立 Epoch 是为了减少共识处理的频率，slashing 或奖励信息等较为重的进程通常在 Epoch 边界进行。Epoch boundary blocks (EBB) -> checkpoints
https://ethos.dev/beacon-chain



### 2024.4.16

Week 2 notes 完成学习：https://ab9jvcjkej.feishu.cn/docx/BRDdd8kP9o00a2x6F4scRo0fnJh

https://learnblockchain.cn/article/4998

### 2024.4.15

Week 2 区块验证和区块构建的概述：

- Function process_execution_payload: 由信标链执行，验证块有效并将 CL 向前移动。CL 进行检查并将 payload 发送至 EL 以进一步验证（通过execution_engine）。https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/beacon-chain.md#modified-process_execution_payload

- Function notify_new_payload: CL 将执行负载发送到 EL，由EL执行状态转换。https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/beacon-chain.md#modified-notify_new_payload

- State transition function (STF): 需要父区块（验证父区块到当前块的转换逻辑）；当前块；状态库（StateDB）（醉后已知的有效状态，存储与父区块相关的所有状态数据）
返回：更新后的 StateDB（包含当前块的信息）；错误。
首先验证 headers，出现如下情况会发生错误：Gas limit 变化超过前一个区块的1/1024；区块编号不连续；EIP-1559基本费用未正确更新。
第二步若 headers 正确，则应用 Tx。覆盖区块 tx 后通过 VM 执行每个 tx，若 tx 正确则更新状态。若有一个无效 tx 则整个块无效，不更新状态。

- Wrap function eg. newPayload：需要执行负载（Execution payload），返回 bool 到信标链，然后信标链会调用 STF。

CL 判断共识，EL 执行共识，CL 有驳回区块的权力


### 2024.4.12

还是没去看 EL 和 CL

今天看到有人拿 Restaking 和08年次贷危机的 CDO 对比，说 Restaking 是最大的雷，重新看了下 Restaking 赛道，虽然 Restaking 赛道并不是 CDO 那样单纯的嵌套流动性，一块砖垒着一块砖垒一座高塔，但是却也是一座座互相关联，有嵌套关系的收益高塔，关键在于 AVS 的实际运行如何。

牛市后期一定会爆掉一些大的 AVS，然后连带着塌掉很多收益高塔。


### 2024.4.11

这两天无心学习，看了 Vitalik 关于 Meme 的文章，制定了 ZKP 的学习计划，以太坊执行层和共识层的学习明天继续。

https://learn.z2o-k7e.world/beginner.html


### 2024.4.9

Week2 EL层细节


### 2024.4.8

Day4：通过 https://ethereum.org/quizzes
测验对以太坊基础知识的了解，查漏补缺之后继续 Week2 的学习。

Gavin Wood 在15年发布以太坊后创立了 Web3 这个名词。
账户和钱包不是一个东西，钱包是通向以太坊账户的界面。

Rollups 扩展以太坊的下一步关键步骤是什么？
将运行 sequencer 和 provers 的责任分配给更多人。

Proto-Danksharding 为 roll-ups 引入了临时数据存储选项，使他们能够更便宜地将结果发布到主网

Question number 2:What hard drive storage is required for an Ethereum node?：2 TB SSD

Using the same client as a majority of the rest of the network puts you at risk for being slashed in the event of a software bug in that client. Running a minority client protects against this.

solo staking 时，仅仅离线并不会导致 slashing。 离线时这将导致轻微的不活动处罚，但返回在线后将恢复证明。

validator 需要正常运行 50% 以上的时间才能盈利，Validators are penalized approximately 75% of what they would have been rewarded for correctly and promptly attesting to the state of the chain. This means for a given time period, being offline 50% of that time will still be net profitable, albeit less profitable than a more reliably available validator.

### 2024.4.7

Day3：Week 0，1 内容学习，旨在查漏补缺，记录之前遗漏的部分。
https://epf.wiki/#/eps/week0
https://epf.wiki/#/eps/week1

Week0内容笔记：

前置知识：What is BitTorrent?  
https://www.youtube.com/watch?v=xH00ikD1oDo

BitTorrent允许所有节点自由传输自己整个文件中拥有的部分，从距离最近，传输最快的地方补全中间没有的部分，这会使得文件在网络中的传输比中心化服务器来得更快。

BitTorrent的上传下载是不对称的，减少单一节点上传的压力，旨在利用所有可利用的上传带宽，因为在网络边缘的节点，人们下载的速度通常比上传的速度快得多。

Wiki上有补充细节：https://zh.wikipedia.org/wiki/BitTorrent_(%E5%8D%8F%E8%AE%AE)

Week1内容笔记：

史前史和 ETH 的基础哲思：UNIX，自由软件运动 FOSS（Free and Open Source Software），非对称加密密码学

以太坊协议设计：白皮书为概述，黄皮书为技术说明，更新在 EIPs (Ethereum Improvement Proposals) 中体现。https://eips.ethereum.org/

更新的基本原则：Simplicity, Universality, Modularity, Non-discrimination, Agility：简单性、通用性、模块化、非歧视性、敏捷性

客户端的实现分为 CL 和 EL 层，细节会在之后详细了解。以太坊的测试和测试标准很重要，之后也会详细了解。

在以太坊中，社区的想法和建议的更改在这里: https://eips.ethereum.org/EIPS/eip-1

讨论核心升级的最集中的地方在这里：https://ethresear.ch/

另一个与 EIP 流程相关的论坛，用于讨论具体提案：https://ethereum-magicians.org/



### 2024.4.6

Day2：今日完成 https://epf.wiki/#/wiki/Cryptography/ecdsa 的学习

Elliptic curves over finite field 有限域上的椭圆曲线
有限域椭圆曲线加法群有良好的循环特性，适合作为密钥生成的场景。
例：Alice 利用如下参数生成了一条曲线
sage: E = EllipticCurve(GF(997),[0,7])
Elliptic Curve defined by y^2 = x^3 + 7 over Finite Field of size 997

在曲线上随机生成点 G，标量相乘 n 次后到达无穷远点 O，n(G) = O
Alice 从 n 中随机选择了 42 作为私钥 K，公钥则为 P 点，P = 42(G) 也就是 P = K*G
sage: P = K*G
(858 : 832 : 1)   P 公钥则为 (858,832)

Alice 发送交易信息时会先将交易信息取哈希值 m，对于每个签名都有临时密钥对的生成减轻暴露的攻击
Randomly selected ephemeral secret key.
sage: eK = 10
Ephemeral public key.
sage: eP = eK*G
(215 : 295 : 1)

Ephemeral key pair =[eK,eP]=[10,(215,295)].
计算签名的组件 s = (k^-1)(e+rK)(modn)
其中 r 是 eP 的 x 轴值，这个签名中同时用到了私钥 K 和 临时密钥对

x-coordinate of the ephemeral public key.
sage: r = int(eP[0])
215
Signature component, s.
sage: s = mod(eK**-1 * (m + r*K), n)
160

签名对是 (r,s)=(215,160) 

Bob在拿到签名对时展开验证，首先验证交易哈希，其次计算临时公钥 R，并以此确认 r。
sage: R = int(mod(m*s^-1,n)) * G  + int(mod(r*s^-1,n)) * P
(215 : 295 : 1)
Compare the x-coordinate of the ephemeral public key.
sage: R[0] == r
True # Signature is valid ✅

哈希变化或者私钥错误，交易都会验证失败，ETH 中的实际情况要比这复杂，详见
https://web.archive.org/web/20240229045603/https://lsongnotes.wordpress.com/2018/01/14/signing-an-ethereum-transaction-the-hard-way/




### 2024.4.5

Start here！从 epf.wiki 的底部目录先开始看吧，我一直没有看过椭圆曲线签名的细节，今儿个来学学数学~~~
https://epf.wiki/#/wiki/Cryptography/ecdsa

ECDSA: Elliptic Curve Digital Signature Algorithm
ECC: Elliptic curve cryptography
交易中需要验证交易中的真实授权，以及信息未被篡改。
以太坊使用的椭圆曲线标准为 secp256k1：http://www.secg.org/sec2-v2.pdf   其中a=0，b=7，y^2 = x^3 + 7

搜到 Secp256k1 作为基于Fp有限域上的椭圆曲线，由于其特殊构造的特殊性，其优化后的实现比其他曲线性能上可以特高30％，有明显以下两个优点：
1）占用很少的带宽和存储资源，密钥的长度很短。
2）让所有的用户都可以使用同样的操作完成域运算。

But Why？不过先放下性能问题，我先搞明白原理

Group：群，Field：域。椭圆曲线有趣在于曲线上两个点形成一个组，两点“相加”的结果保留在曲线上。这是一种几何加法，通过绘制选择的两个点为一条直线并在 x 轴上取曲线交点为和。
椭圆曲线加法群：定义为椭圆曲线在实数域上的点集以及点加法。

当两点重复时，所绘制的为 P 点切线，在这种情况下，直线将成为反映总和 (2P) 的 P 的切线。
Repeated point-addition is known as scalar multiplication:重复点加法称为标量乘法
nP = P + P + P + P（加 n 次）


Discrete logarithm problem 离散对数问题
标量乘法可以用作生成密钥
私钥表示为 K，代表基点 G 自相加的次数，最终产生公共点 P
P = K * G
我们要确保标量乘法不会泄露我们的私钥，这要确保标量乘法“容易”，且“不可追踪”。
模运算引入环绕效应，对于素数模，这是特别困难的，被称为离散对数问题。
