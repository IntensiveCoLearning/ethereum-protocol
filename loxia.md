## loxia

哈喽大家，我是 loxia.eth，来这里补课学习一些基础且重要的知识，很高兴能来到这里和大家一起学习。

## Notes

### 2024.4.26

ERC-3525

半同质化代币标准，此代币（SFT）实现了可计算、自描述、兼有同质化代币和非同质化代币特性。SFT重新定义了数字资产的生成和交易，提升市场效率和诚信度，应用范围广泛，例如礼品卡、支票、代金券、债券、期货、期权、ABS、房地产、能源等。

数据结构包括tokenID, value, slot metadata：

1）tokenID：与ERC-721标准中的tokenID一样，代表非同质化、独一无二的所有权；

2）slot metadata：用于描述资产的各种属性的集合，比如公司债券的属性有面值、到期日、收益率等等，这部分代表了同质化或非同质化特性；

3）value：与ERC20中的balanceOf()/_value类似，描述一个ERC-3525代币中各底层资产的数量，如果多个代币的slot属性相同，value可在代币间转移；

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95400362/d7e39154-5bc7-41d6-9bb6-57b55650a3f2)

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95400362/70853ac1-6977-4f0c-9a03-3fc587a196b8)

https://github.com/solv-finance/erc-3525/blob/main/contracts/ERC3525.sol

周六周日 todo：详细了解3525


### 2024.4.24

看一下 RANDAO 的代码，还有 PBS (proposer-builder separation) 的细节。

### 2024.4.23

https://github.com/randao/randao

RunDAO: 真随机数在编程中非常重要！ 确定性系统中的 RNG（ Random Number Generator ） 非常困难。

RanDAO 定义了参与规则（RANDAO contract）。生成随机数的基本过程可以分为三个阶段：

第一阶段：收集有效的 sha3(s) 
任何想要参与随机数生成的人都需要在指定的时间段内（例如 6 个区块周期，大约 72s）向合约 C 发送一笔交易，并以 m ETH 作为质押， 伴随着 sha3(s) 的结果，s 是参与者各自选择的秘密数字。

第二阶段：收集有效s 
第一阶段结束后，任何成功提交sha3(s)的人都需要在指定的时间段内向合约C发送带有第一阶段秘密数字s的交易。 合约 C 将通过对 s 运行 sha3 并将结果与之前提交的数据进行比较来检查 s 是否有效。 有效的s将被保存到种子集合中，最终生成随机数。

第三阶段：计算随机数，退还质押的ETH和奖金 
成功收集到所有秘密数后，合约C将根据函数f(s1,s2,...,sn)计算随机数，结果为 写入C的存储，结果将发送给之前请求随机数的所有其他合约。
合约C将质押物返还给第一阶段的参与者，并将利润分成等份，作为额外奖励发送给所有参与者。 利润来自于消耗随机数的其他合约所支付的费用。


合约的参数可以由不同的需求而更改，来满足不同级别的安全需求。Readme 中举了一个博彩 Dapp 的例子，介绍了作恶者分散质押参与以减少作恶成本的做法，并引入了在该情况下没收参与 RanDAO 计算而质押的 ETH 的规则，后面介绍了 RanDAO 会员制作为额外系统来防止此类攻击，参与者通过缴纳会费来获得该等级的会员，在违反规定时，会费会被罚没。

Readme 中的 QA：

问：为什么不让矿工参与 RNG？为什么不使用 tx 哈希值、nonce 和其他区块链数据？
答：矿工有能力操纵这些区块链数据，因此可以间接影响 RNG。如果 RNG 包含区块链数据，矿工就有能力构建对自己有利的随机数。

问：矿工可以忽略某些包含他们不喜欢的随机数的交易，如何处理？
答：这就是为什么我们需要一个时间窗口期。合理的时间段应大于 6 个区块，我们相信没有人能连续产生 6 个区块。因此，如果参与者诚实，只要每个时间窗口打开，他就会立即发送号码，就不用担心被排除在外。

问：为什么使用所有参与者的所有号码，而不是一个子集？
答：选取子集的规则是确定性的，因此参与者会尝试通过各种方式在集合中占据指定位置，如果他们成功了，他们就会事先知道随机数是从子集中产生的。如果抽取子集的规则是随机的，那么我们仍然会遇到真正随机的问题。

问：认捐的会费用于何处？
答：将捐给慈善机构或 RANDAO 以维持资金。

注：f(s1, s2, ..., sn) 是具有多个输入的函数，例如 r = s1 xor s2 xor s3 ... xor sn，或 r = sha3(sn + sha3(sn-1 + ... (sha3(s2 + s1))))


### 2024.4.22

研究下 RANDAO 细节和真随机数的生成，还有 PBS (proposer-builder separation) 的细节。结束 Week 2 的学习


### 2024.4.21

[DAOs, DACs, DAs and More: An Incomplete Terminology Guide | Ethereum Foundation Blog](https://blog.ethereum.org/2014/05/06/daos-dacs-das-and-more-an-incomplete-terminology-guide)

**DAOs == automation at the center, humans at the edges.** 

从 DO 到 DAO，关键在于自动化能力。

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f9e21f4a-a137-4782-94dd-8f61c319e242/22288f8f-93eb-445a-9283-f94128b035e9/Untitled.png)

自动化在中心（程序正确），人在边缘（人不被抛弃，人的思想和行为仍占据主导，慎用 AI）

**“在一个人类观察比以往任何时候都更具有预见性的时代，在对未来进行编程时，学习如何将社会性编入系统 (而不是基于信任编程) 似乎是人类在这个星球上生存下去的必修课。”**

DAO的七个根定义：

***1，元叙事：Metanarrative***

Why us？Why me？Why you？Why xxx？How we work？How xxx？

***2，去中心：Decentralization***

不受单一节点控制，独立，自治。

***3，自主权：Autonomy***

**自主/自动/自治，**DAO所体现的自治本质，事实上是一种对公共资源/公共资产/公共资本进行决定的权力。**并不是**提案-投票的完全/混合民主制（太表面了），而是基于集体自主权的自动化治理，

***4，组织体：Organization***

DAO 的生产关系网络/组织架构，要具备反向构建现实生活空间的能力。

***5，互通性：Interoperability***

不具备任何互通性的 DAO 是 DO，互通性是衡量 DAO 的重要标准（链上互通性和链上-链下互通性）

***6，空间性：Spatiality***

需要有直观的空间特性。DAO 会基于生产关系创造独特的空间，DAO既是组织，也是空间。更明确的说，DAO是具备空间结构特征的组织体。

***7，二重性：Duality***

既是社会结构的二重性，亦是指符号穿透现实的二重性。

DAO 会有由编程语言和特定文本规则，围绕 DAO 内独有的资源和生产关系，构建出新的一套社会结构，有着新的语境和规则。

通过重构现有的符号和规则去构建新的社会结构，使得人们可以超越经典范式去理解新世界。

**最重要的，能确保人类在新世界中不迷失最重要的思想。**

DAO 的形成不只依赖于现实物质条件的支撑，**其根本性来自于社会生存文化的叙事，**在多元价值观的时代，每个人都可以选择自己的所喜好的生活方式，背后是生存文化的影响。

对于 DAO 来说，协议的底层逻辑和服务设计的框架是最重要的，数据资源是 DAO 中权力来源（SBT），DAO 需要利用好该来源，DAO 切勿只把目光放在业务工具本身上。

叙事通常包括五个要素：人物、情节、背景、目的和意义。

### 2024.4.20

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95400362/f534a2b7-bed3-4734-bd54-1ecddef2ba6c)

上图展示了诚实行为如何改变以实施 reorg 策略。

在这种情况下，让 b1 代表晚块。由于迟到，b1 只拥有时隙 n 的证明权重的 19%。其余 81% 的证明权重分配给父块 HEAD，因为许多证明者在证明截止日期前没有看到 b1。

如果没有 honest reorgs，slot n+1 的提议者会将 b1 视为链的头部并构建子块 b2。提议者没有努力 reorg b1，尽管它只有 19% 的证明权重。但在 slot n+1 期间，b2 具有 proposer boost，并且假设它按时交付，则 b2 将通过积累该 slot 的大多数证明而成为规范。

如果有 honest reorgs，情况就会大不相同。因此他们构建一个以 HEAD 作为 b2 的父块，并强制 reorg b1。当我们到达 slot n+1 的证明截止日期时，honest reorgs 将比较 b1 (19%) 与 b2（来自 proposer boost 的 40%）的相对权重。所有客户端都实施了 proposer boost ，因此 b2 将被视为链的头部，并将累积 slot n+1 证明。

以上过掉 Slot 相关

Finality：最终性，意味着 tx 是块中无法更改的一部分，当一个 epoch 结束时，若其 checkpoint 已经聚集了2/3的绝对多数，该 checkpoint 就被合理化了（Justification），合理化之后会被最终确定（Finality）。

原本 validator 需要 1000 ETH，但是 Justin Drake 建议使用 BLS 签名技术，将最低资本要求降低至 32 ETH。

### 2024.4.18

Slot 和 POS 交易排序详解：https://www.paradigm.xyz/2023/04/mev-boost-ethereum-consensus

如果 Validators 在 t = 4 的证明截止时刻结束前没有看到新的块，将投票给先前的链头，区块提出的越早，传播的时间就越多，因此积累证明就越多。从网络健康的角度来看，发布区块的最佳时间是 t=0（根据规范规定），然而，由于区块的价值随着时间单调增加，提议者有动机推迟区块的发布，以积累更多的 MEV，细节详见：https://ethresear.ch/t/timing-games-in-proof-of-stake/13980

从历史上看，只要下一个 validator 在为下一个 slot 构建块之前观察到该块，提议者就可以在证明截止时刻之后很久（甚至接近 slot 结束时）发布一个块。这是子块继承父块的权重以及分叉选择规则终止于叶节点的结果。因此，推迟区块发布并没有什么坏处。为了帮助推动理性行为（延迟区块发布）转向诚实行为（按时发布），实施了“诚实重组”（“honest reorgs”）。

共识客户端引入了两个新概念，这对证明截止日期具有重要影响：proposer boost (https://github.com/ethereum/consensus-specs/pull/2730) ；honest reorgs (https://github.com/ethereum/consensus-specs/pull/3034) 

proposer boost：尝试通过授予提议者相当于完整证明权重 40% 的分叉选择“boost”来最大程度地减少 balancing-attacks（https://arxiv.org/abs/2009.04987）攻击。重要的是，这种 boost 仅持续该 slot 的持续时间（12s）。

honest reorgs：接受 proposer boost，并允许诚实的提议者使用它来强制重组证明权重低于 20% 的区块。（非强制更改）

在这些特殊情况下会避免 honest reorgs：during epoch boundary blocks; if the chain is not finalizing; if the head of the chain is not from the slot prior to the reorged block

条件 3 确保 honest reorgs 仅从链中删除单个块，这充当断路器，允许链在极端网络延迟期间继续生成块。这也反映了 proposer 对其网络观点的信心下降，因为他们不再确定他们的 proposer boost 的区块将被视为规范。


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
