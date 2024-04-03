# BruceXu

Hi guys, I'm Bruce, I'm learning Ethereum Protocol. I'm good at Web development. Follow me on Twitter: <https://twitter.com/brucexu_eth>, my telegram: <https://t.me/brucexu_eth>.

## Notes

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
