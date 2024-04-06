# BruceXu

Hi guys, I'm Bruce, I'm learning Ethereum Protocol. I'm good at Web development. Follow me on Twitter: <https://twitter.com/brucexu_eth>, my telegram: <https://t.me/brucexu_eth>.

## Notes

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
