# BruceXu

Hi guys, I'm Bruce, I'm learning Ethereum Protocol. I'm good at Web development. Follow me on Twitter: <https://twitter.com/brucexu_eth>, my telegram: <https://t.me/brucexu_eth>.

## Notes

### 4.9

TODO

- https://epf.wiki/#/eps/week1 把官方资料过一下，避免太发散了，不要看太多，Week1 的完成
- https://twitter.com/EIPFun/status/1759938858286776710 路线图大概了解几个关键阶段和节奏

### 4.8

发现了个宝藏博主，把以太坊节点在本地电脑上跑起来了。教程：https://blog.wssh.trade/posts/ethereum-node/。

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/0cbf9fb6-6a63-4a87-9287-3614ca63c273)

还需要两天时间完成同步。

#### [Week1](https://epf.wiki/#/eps/week1)

PDF 课件资料：

- 以太坊设计思想：Unix philosophy, FOSS, Cryptography and Cypherpunks
- Cypherpunks：Cypherpunks write code... Our code is free for all to use, worldwide. We don't much care if you don't approve of the software we write. We know that software can't be destroyed and that a widely dispersed system can't be shut down. ——Eric Hughes, Cypherpunk Manifesto, 1993
- Cryptoanarchy：Just as the technology of printing altered and reduced the power of medieval guilds and the social power structure, so too will cryptologic methods fundamentally alter the nature of corporations and of government interference in economic transactions. Combined with emerging information markets, crypto anarchy will create a liquid market for any and all material which can be put into words and pictures. —— Timothy C. May https://activism.net/cypherpunk/crypto-anarchy.html
  - 看 The Crypto Anarchist Manifesto 加密无政府主义者宣言，莫名其妙联想到了星灵的黑暗圣堂武士，因为拒绝加入卡拉（主流思想）被放逐，练就了独特的能力，关键时刻回归拯救了世界。

预习资料：

- 交易的工作流程：操作数据 + 签名 发给 EVM 运算得到结果，更新到 Account Storage，之后被 CL attest 加入到 blockchain 上面 ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/cee81703-2825-41f4-97ef-b8bb89759cd8)
- 一个 Block 最多容纳 3000 万 gas
- gas fee 计算公式 = (base_fee + priority_fee) \* gas_used
  - base_fee 就是基础的平台 gas，交易越多越高，反映了整个平台的
  - priority_fee 是小费，你愿意多给，交易就可以越快打包或者被选中
  - gas_used 就是具体使用的量，比如一个 tx 是 21000 gas，每一个指令或者存储都会有相应的 gas 用量
    - 每个 EVM 字节码的 gas 消耗 https://www.evm.codes/?fork=cancun
    - Editing a storage slot costs 5000 gas, 20000 if slot not filled yet（TODO slot 是如何存储的？）
    - 存储一个 byte 花费 16 gas，如果是 zero byte 是 4 gas
  - 如果很难理解这几个关系和概念，可以把 gas 想象成汽油，每次出行根据需求和复杂度路途不一样，烧的汽油不一样，加油的时候油价是浮动的，根据供需进行调整。如果你比较紧急不想排队，也可以多出价格高价买油。
  - TODO 写一个小科普 Max priority fee, Max fee, Gas limit 的关系和区别
- Full transaction object ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/5abe2fe1-bd1a-4619-ad09-03c595c80a30)
- ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/931e19e5-5cbd-4254-8412-a7a8138932f9)
- ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/55ae4d3a-b425-459f-8999-d165ca0ce633) TODO attest 过程是怎么样的？
- ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/6fcd89fd-ab19-45e7-a61d-f1b890df173c) 简单的应用马上就可以确认，复杂的比较重要的应用，可以等 2 个 epoch 确认
- ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/44afef0b-0727-4fe7-bdbd-3bb5d4cb616c)

TODO Next steps in the Purge https://notes.ethereum.org/@vbuterin/purge_2024_03_31

### 4.7

#### [Week1 Notes](https://twitter.com/EIPFun/status/1759938839890522603)

UNIX 的设计理念、FOSS 的开源思想都是很值得一看的视频，尤其是很多刚刚接触技术或者开源的。这一点在国内的技术学习资料上面很少见。简单的说就是模块化、爱自由、无许可、开放开源等。

TODO 深入学习了解下 Cypherpunks movement 这个，密码朋克宣言等 <https://www.activism.net/cypherpunk/manifesto.html>。这个是加密文化和精神层面的洗礼。

以太坊的最初设计和定位就是 A Next-Generation Smart Contract and Decentralized Application Platform，就是做一个实用的通用的区块链平台，上面可以承载各种去中心化应用，构建一个新世界。

因此，以太坊的构建需要遵循一些原则和文化，比如模块化、可信中立、开放开源、比较平台和中立，不做定制等等。以太坊的升级流程也是通过 EIP（社区驱动，不断的讨论、修补，定稿） -> 客户端落地实施升级。比较大的改动都会通过硬分叉的方式实现。

TODO 实际升级的时候如何实现某个时间点切换的？例如最近的 Dencun 升级，背后的升级操作是怎么样的？按照目前我的思路，客户端需要改造升级，然后支持 Blob 这个存储，之后节点升级客户端版本重启之类的才可以实现新的功能吧？还是提前升级好了，到时候有个开关切换？

TODO 以太坊的 Validator 是不能停机的，如果不提供服务将会导致扣费。那么 Validator 的升级是如何进行的？

以太坊的模块化设计，是从 PoW 转成 PoS 开始的，目前的架构：

- Execution Layer：运行智能合约、存储 Accounts State、运行处理交易
  - EVM：Ethereum Virtual Machine，就是智能合约运行虚拟机，可以简单类比 Docker Container 或者 serverless，对被调用的智能合约代码创建一个虚拟机进行代码运行，并且输出相应的结果或者改造到 The World State
  - State (data) & Transactions：具体的以太坊区块链上存储的状态和数据
  - P2P layer：网络的节点信息交互和传输
  - 对外接口：
    - JSON-RPC API：可以直接暴露给外部的一些 Dapps
    - Engine API：用于跟 Consensus layer 进行同步和交互 canonical block（TODO canonical block 是什么）
  - 实现：Geth、Nethermind、Besu、Reth、ethereumjs
- Consensus Layer：包括共识算法选举块的逻辑、分叉选择、Block gossip（TODO）
  - Fork choice：Fork choice 算法会选出来一个合适的链，具体算法是 LMD GOHST (Latest Message Driven - Greedy Heaviest Observed SubTree)（TODO 了解下逻辑）
  - P2P layer：跟其他 consensus 客户端交互，实施 blocks gossip，选出一个 global ledger 所有人都同意
  - Blobs：EIP-4844 升级之后，存储的内容
  - RANDAO：TODO
  - Validator：是一个运行在 Consensus 客户端上的插件，支持 staking 来获得激励
  - 对外接口：
    - Beacon API： 跟 Validators 进行沟通和
  - 实现：Prysm、Lighthouse、Teku、Nimbus、Lodestar

一个以太坊完整的支持质押的 full node 上面需要运行 Execution client、Consensus client 和 Validator。通常是需要同时运行，听 Kenway from XHash 的说法，有一些算法的限制，防止不在同一台服务器上运行。开发者常用的查询以太坊信息、智能合约状态的服务其实就是通过 RPC 连接到了一个 Execution 客户端查询的。

**Testing in Ethereum**

很多客户端或者工具，测试挑战挺大的。包括比较常见的性能测试、压力测试、单元测试之类的。

工作流程通常是：提出想法 -> 研究 -> 产出标准（EIP）-> 实施 -> 测试 -> 客户端升级更新。非常缓慢和漫长。

通过 AllCoreDevs call、论坛 EthMagicians、Discord EthCatHerders 来沟通。当然未来还有 EIP Fun 这个社区和项目。

**以太坊路线图**

Vitalik 会周期性的更新，基本包括了整个以太坊的发展阶段以及接下来的一些重点工作等。

关键词和概念：

- Gossip Protocol <https://www.cs.umd.edu/~dml/papers/ethereum_fc21.pdf>
- LMD GHOST
- Ethereum Roadmap <https://domothy.com/roadmap/>
- Epoch：1 Epoch = 32 slots or 6.4 minutes
- Slot：一个 block，大概 12s 出一个

**World State vs Accounts State：**

World State 包含了整个以太坊网络的全部 state，是一个 mapping（TODO 有待验证或者找到原始数据），映射了所有的 Accounts 和 Contacts。通过 Merkle Patricia tree 数据结构来存储（TODO 存储位置？）。一个全新的以太坊钱包地址是没有办法在 World State 上被找到，因为之前没有过交易，所以不会被存储，否则这个钱包地址几乎无限，会导致 World State 太大。因此有些场景需要钱包进行至少一次交易才可以。

Accounts State 就是比较具体的到某个地址的存储数据，包括 storageRoot, Balance 等等，后面详细介绍吧。

除此之外，以太坊还包括 Transaction、Block 和 Blob 等数据，下图是一个简单的关系图（没有包括升级后的 Blob）。

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/24812a47-3032-4058-8af1-67bda046abd6)

Credit: <https://www.lucassaldanha.com/author/lucas-saldanha/>

TODO [Lucas Saldanha](https://www.lucassaldanha.com/author/lucas-saldanha/) 的博客有很多不错的图示，比较适合参考绘制以太坊

#### Ethereum roadmap

TODO https://twitter.com/EIPFun/status/1759938858286776710

#### Merkle Patricia tree

TODO 介绍下这个数据结构。这个数据结构用来存储 Ethereum World State，然后存储和验证数据比较高效。

TODO 不错的介绍文章：

- https://medium.com/cybermiles/diving-into-ethereums-world-state-c893102030ed
- https://docs.dogechain.dog/docs/concepts/ethereum-state

#### Trie 数据结构

TODO 还有这个数据结构介绍一下。

### 4.6

Week0 预习视频笔记：

#### [What is BitTorrent?](https://www.youtube.com/watch?v=xH00ikD1oDo)

传统的软件下载方式，就是正常的有个服务器，大家去下载：

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/1e9b1b44-526d-462d-a3a7-26db83d8dd88)

问题很明显，可能会比较慢，对这个服务器也有比较大的成本和压力。

BitTorrent 就是每个电脑都保存一些数据，一开始比较慢，其他人会先下载一个片段，之后再交叉同步。所以会越来越快，越多人下载和分享就会越快。但是每台电脑要开放自己的宽带进行上传，要有分享精神。

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/29a0764a-6101-4fd9-b874-38e39ec197a1)

#### [What is a Peer to Peer Network? Blockchain P2P Networks Explained](https://www.youtube.com/watch?v=ie-qRQIQT4I&ab_channel=Lisk)

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/cebb512a-578f-4a97-8e35-4f121604dde2)

简单的说就是 P to P 直接链接和进行信息交换，避免了中心化的中转和单点的控制。信息也会被保护的很好，不容易被篡改或者丢失。

#### [But how does bitcoin actually work?](https://www.youtube.com/watch?v=bBC-nXj3Ng4&ab_channel=3Blue1Brown)

比较简单和基础吧。没看过的看看就行。

### 4.5

#### [Proposer-Builder Separation 详解](https://mp.weixin.qq.com/s/Lhn-Lu0IIiOg2mR0_4T0qw)

接昨天的 Part 3。

第一阶段：PBS 设计

PBS 有 5 点设计要求：

1. Untrusted proposer friendliness: 一个 PBS 机制不应该依赖于某些受信任的 proposer。
2. Untrusted builder friendliness: 同样，它也不能依赖于 builder 不会作恶。
3. Weak proposer friendliness: 它不对 proposer 有过高的要求（设备好，带宽大等要求）
4. Bundle unstealability: proposer 不能从 builder 构建好的包中，提取交易来构建自己的 block body。
5. Consensus-layer simplicity and safety: 不能损害共识层。

第一阶段主要是实现 PBS 的需求。在 Proposer 选择和广播的方式上稍微有一些区别，对应也有共识层的复杂度或者宽带需求等问题。

第二阶段：mev-boost 实现

通过新增 Relay 和 searcher 两个角色解决。TODO 咋解决的？

第三阶段：enshrine Proposer-Builder Separation (ePBS) 设计

TODO Vitalik 提出的 Two-Block HeadLock (TBHL) 研究一下。TODO <https://ethresear.ch/t/why-enshrine-proposer-builder-separation-a-viable-path-to-epbs/15710>

#### https://epf.wiki/#/

EPF Protocol Studies 的目的就是因为以太坊协议的需求量增加，需要不断的有一些才能的人加入到 core level 的级别。这个 EPS 就是来提供教材和填补这个空缺的。

10 周的课程，两个阶段。前半段是协议的运行机制、架构、概念。第二部分就是两个方向：开发和研究。都是核心开发者和研究员来带课。

EPS 计划目的之一也是一起构建一个不错的 wiki，方便未来大家自学。可以赶紧贡献一下。这个课程不涉及到应用层的 Solidity 开发。

#### https://epf.wiki/#/eps/week0

视频的内容基本上可以看 <https://twitter.com/EIPFun/status/1775443183884763521> 这里的笔记。

发现了问题如何贡献 Wiki 呢？

[贡献指南](https://epf.wiki/#/contributing) 简单的说：

- 避免重写或者 copy，优先使用链接的方式
- 仔细阅读一下贡献规范等，代码仓库在 <https://epf.wiki/#/contributing>
- 提交到 main 分支上，等待 merge
- 不要放一些 dapp、l2、rollups 之类的无关信息，内容会被 EIP 覆盖，而不是 ERC

具体的一些目录格式就具体自己看吧。不要用 LLMs 来生成，质量要好一些。强行硬蹭了一个 PR <https://github.com/eth-protocol-fellows/protocol-studies/pull/149>，发现了一个 URL 错了。

基础知识简单概述一下，我大概都知道一些：

- Cryptography
  - Hashing：哈希算法，简单的说就是通过一个算法可以对任何内容生成一个字符串，不可逆转而且尽量追求不能碰撞。比较简单的模型是求余数算法，但是碰撞的几率非常高。如果一旦可以碰撞，这个 hash 算法就不太安全了。
  - Public key cryptography：不对称公钥私钥加密算法，具体流程就在这个图
    ![](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/537eac62-183c-4284-8451-d0235cf2db7a)
- Data structures
  - Merkle trees：就是每个节点带有叶子结点内容 hash 的一棵树，好处就是很容易校验得出某个节点的内容是否被篡改过。因为内容变化之后，Hash 得到的字符串会发生变化。
    ![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Hash_Tree.svg/2560px-Hash_Tree.svg.png)
- Networking, p2p and distributed systems：分布式网络，包括信息互相传播等等算法
- Software development basics
  - Programming languages, compilers
- Ethereum as a platform
  - From a user perspective：通过钱包工具调用 RPC 发送相关的交易，通过签名来确认是自己签发的操作，之后进入网络后进行广播
  - As a dapp developer：编写智能合约，部署上主网，进入到以太坊各个节点，state 存储到 World State 里面。

### 2024.4.4

#### [Proposer-Builder Separation 详解](https://mp.weixin.qq.com/s/Lhn-Lu0IIiOg2mR0_4T0qw)

- “在 proto-danksharding 中，一个 Block 扩容 3 个 blob 不够，在 full-danksharding 中会进一步扩展。( Target: 16 MiB, Max: 32 MiB )”。不是最多可以到 6 个 blob 吗？
- Validator 对 block 和 blob 的 Attest 的工作流程是怎么样的？相关代码在哪里？
- 以太坊 PoW 到 PoS 我目前的理解是节点和服务器还是挺多的，只不过不需要每一台服务器都需要大量的运算挖矿，但还是需要开机提供服务，为什么能源消耗可以缩减这么多呢？
- DAS 的工作原理是 validators 去抽样、保存部分 blob 到服务器上，相对均匀一些，合并之后是可以还原的。但理论上确实会存在丢数据的可能性，所以降低了安全性。
- PBS 是 proposer builder separation，主要用于增强 transactions censorship resistance <https://notes.ethereum.org/@vbuterin/pbs_censorship_resistance>
- 核心思想是两种角色分离：builder 负责创建和广播 block body（消耗大，硬件高）、proposer 负责创建和广播 block header，选取一个 body 添加到 header 里（消耗低，硬件低）
  - TODO header 的部分内容不是根据 body 生成的吗？可以随便选择一个 body 加入 header？需要看看代码和找到 raw data 了解下
- PBS 的出块步骤（以 Hybrid PBS 为例）：
  - step1 Proposer 先把它的 Mempool 中的所有交易公布出来，组成一个 [crList](https://notes.ethereum.org/@fradamt/H1ZqdtrBF) (censorship resistance list) ，广播给所有的 Builder。
  - step2 每个 Builder 从 crList 中选取交易进行排序，直至填满 Block 的 Gas Limit。Builder 无法插入自己的私有交易来获取 MEV，因为交易必须广播给 Proposer。Builder 也无法拒接打包某个交易，因为要填满 Gas Limit
    - TODO 目前如何通过插入私有交易来获取 MEV？
  - step3 所有 Builder 都公布自己最终排序出来的交易列表的 Hash，并发送给 Proposer，Proposer 从中选取一个 Hash，填入 Block header 并广播。
    - 解决了上面的 TODO，Hash 还是根据 body 由 builder 生成，Proposer 去从生成列表选一个就行
  - step4 其他节点从 Proposer 处同步 Block header，从 Builder 处同步 Block body
- 大概思路搞清楚了，把工作内容分层，硬件要求高的其实会造成中心化因为能跑的人少了，因此将该部分工作内容设定为只能加工和处理，不涉及到 attest 或者被选中的逻辑。选择 Block Body 的 hash 的工作相对轻松，所以能跑的人很多，甚至可以放在手机等。通过广泛的去中心化来增加抗审查特性。其实就是传统的架构设计的思路，要么分层要么模块化，高内聚低耦合，针对不同特性和需求进行拆分合并。比较独特的就是区块链的数据结构和独特的工作流程（P2P），这个在架构拆分的时候，会遇到一些不一样的地方和需要处理的。掌握这块领域知识应该就差不多了。

TODO 没看完，没时间了。

#### [State of research: increasing censorship resistance of transactions under proposer/builder separation (PBS)](https://notes.ethereum.org/@vbuterin/pbs_censorship_resistance)

- block proposer 是 miner or validator。会从 mempool 里面找到最高 priority fee 的交易打包，所以有一些操作的空间，比如打包或者不打包某些交易。那其实也可以实现一些黑白名单审查机制，比如不打包来自某些拉黑地址的交易。
- TODO 了解下 MEV 的工作原理。
-

#### [Forward inclusion list](https://notes.ethereum.org/@fradamt/forward-inclusion-lists)

TODO

### 2024.4.3

拖延了两个仓库，终于创建好了这个仓库。这几天重新看看清理一下官方 EPS Discord <https://discord.gg/FwtAGPMD6p> 的消息。顺便把一些有价值的对话贴过来吧。
