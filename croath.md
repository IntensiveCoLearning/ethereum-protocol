# Croath

Hi 👋 guys, you can call me 小鱼. I'm interested in Etheruem Protocol learning. I have expierence in backend/frontend/smart-contract dev.

 - Twitter/X: <https://twitter.com/cr0ath>
 - GitHub: <https://github.com/croath>
 - Telegram: <https://t.me/croath>

## Notes

### 2024.4.3

刚刚入门。

### 2024.4.8

学习资料：

 - https://epf.wiki/#/eps/week0
 - https://epf.wiki/#/eps/week1

读了 week0 和 week1 的前导资料和学习内容，大部分都是已经了解的知识。PoS 之后没太关注过节点架构，所以主要学习了 [node architecture](https://ethereum.org/en/developers/docs/nodes-and-clients/node-architecture/)，明天准备把整个[章节](https://ethereum.org/en/developers/docs/nodes-and-clients/)都看完，方便之后进行 week2 的学习。[以太坊白皮书](https://ethereum.org/en/whitepaper/#ethereum-whitepaper)也开始重看，刚好配合 week2 的内容同步学习，希望三天内能够看完。

### 2024.4.9

学习资料：

 - https://ethereum.org/en/developers/docs/evm/opcodes/
 - https://soliditylang.org/blog/2024/01/26/transient-storage/

学习了 evm 中的基本 opcode 和相应的 gas 计算方式，深入研究了 `TSTORE` 和 `TLOAD` 的使用方法以及相应对 transient storage 操作的方法，研究了关于重入攻击保护的应用例子代码。

### 2024.4.10

学习资料：

 - https://ethereum.org/en/developers/docs/gas/

以前的 gas fee 计算方式比较了解，这次学习了 1559 之后的新 gas fee 计算方式。但是文档也有不全面的地方，例如文档中说：

```
Every block has a base fee which acts as a reserve price. To be eligible for inclusion in a block the offered price per gas must at least equal the base fee. The base fee is calculated independently of the current block and is instead determined by the blocks before it - making transaction fees more predictable for users. When the block is created this base fee is "burned", removing it from circulation.

The base fee is calculated by a formula that compares the size of the previous block (the amount of gas used for all the transactions) with the target size. The base fee will increase by a maximum of 12.5% per block if the target block size is exceeded. This exponential growth makes it economically non-viable for block size to remain high indefinitely.
```

这里对 base fee 的 target size 和 12.5% 的提升数字描述还是比较模糊的，具体的解释应该是：

```
在 Ethereum 的 Gas 费用模型中，"target block size" 是指开发者和网络共识设定的一个理想的区块大小，用于平衡网络的吞吐量和区块生产速度。这个目标区块大小通常是区块容量上限（或者说是"gas limit"）的一半。这种做法的目的是为了让网络自动调整交易费用，以响应网络拥堵情况。

至于为什么会有最多12.5%的增长率，这个数字是基于对网络安全性、可用性和费用可预测性之间权衡的结果。通过限制每个区块基础费用的最大增长率，Ethereum 尝试在交易费用的可预测性和对网络拥堵的响应之间找到平衡点。这个比率被设计为既足够快，可以在网络使用量急剧上升时迅速调整费用，又足够慢，以避免因为短期的使用量波动导致的费用剧烈变化，从而保持网络的经济安全性和用户费用的可预测性。

简而言之，"target block size" 是一个设计上的决定，用于指导基础费用的自动调整机制，以维护网络的高效和公平使用。而12.5%的增长上限是经过计算和权衡后认为既可以有效应对网络拥堵，又不会引起费用的剧烈波动，从而保证了网络的稳定和用户体验。

以太坊（Ethereum）网络的目标区块大小是由网络的“gas limit”决定的，具体而言，目标区块大小通常被设定为一个区块 gas 上限的一半。以太坊的区块 gas 上限是动态调整的，由矿工（或在以太坊 2.0之后，是验证者）投票决定。这意味着目标区块大小并非一个静态数字，而是随着区块 gas 上限的调整而变化。

至于12.5%的增长率上限，这个数字是在以太坊EIP-1559提案中固定设定的，不是动态调整的。EIP-1559是一次重大的网络升级，旨在改善以太坊的费用市场机制。其中引入的基础费用（base fee）机制，就包括了这个最大12.5%的调整率。这意味着，如果一个区块的使用超过了目标区块大小，下一个区块的基础费用会最多增加12.5%。相反，如果区块使用量低于目标大小，基础费用会相应减少，但减少的比例也受到一定的限制，以确保网络费用的平稳变化。

这个12.5%的上限是设计上的决策，目的是防止在短时间内因为网络拥堵导致的交易费用剧烈波动，从而增加费用的可预测性，同时保持网络的安全性和稳定性。
```

单区块 gas 上限目前是 30m，不过 V 曾经在一次 reddit AMA 中提议提升 33.3% 到 40m。这一提议遭受了很多反对意见，比方说很多 node runners 表示这样会大幅增加运营成本，社交媒体上也曾有很多人表示这样的提议很不负责，如果 40 不够是要改成 80 吗，那不如直接改成超大。目前还没有增加 gas 上限的新投票。相应的投票实际上不是实时的、动态的，一般来说不会经常变化，但是理论上确实发生在每次出块的过程中。

### 2024.4.11

学习资料：

 - https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/

学习了 Merkle-Patricia 的基本原理，以及相对基本的 Merkle tree 的改善。相应的算法和代码均已学习。

### 2024.4.12

学习资料：

 - https://epf.wiki/#/eps/week2
 - https://cs251.stanford.edu/lectures/lecture7.pdf
 - https://www.youtube.com/watch?v=7sxBjSfmROc
 - https://epf.wiki/#/eps/week3
 - https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/


 > 注：[视频 An Overview of the Ethereum Excecution Layer - Dan Boneh](https://www.youtube.com/watch?v=7sxBjSfmROc) 中 5:40 左右对交易执行和同步区块的顺序解释有误，视频下的讨论也没有结论，tx 在用户端不是先进入 consensus layer 的。正确的顺序是：  
 > 1. 交易的提交：用户提交的交易首先进入到执行层（Execution Layer）。在执行层，交易被放入交易池（mempool）中等待被处理。这是因为执行层负责管理以太坊的状态和执行交易（包括智能合约的执行）。
 > 2. 区块的提议：共识层（Consensus Layer）根据权益证明（PoS）机制选出一个验证者（validator）来提议新的区块。这个验证者会从执行层的交易池中选择交易来构建新的区块。
 > 3. 新区块的处理：验证者在共识层提议新的区块后，通过 `notify_new_payload` 函数将这个新区块（或称为payload）发送给执行层。执行层接收到新区块后，会对其中的交易进行处理和验证，包括执行智能合约、更新状态等。
 > 4. 区块的确认：一旦执行层完成了对新区块的处理并验证无误，共识层就会最终确定这个区块，并将其加入到区块链中。这样，交易就被确认了。

### 2024.4.13

学习资料：

 - https://ethos.dev/beacon-chain

The Beacon Chain:

1. 一个 slot 是 12s，一个 epoch 有 32 个 slots，也就是 12s *32 = 384s = 6.4mins
2. 一个 slot 一般有一个 block，但是由于 validator 下线等原因也可以是空的
3. Validator 的最大 balance 是 32E，但是 staking 可以很多
4. 每一个 slot 都至少有一个 committee，每一个 committee 至少有 128 个 validators，一个 epoch 里每个 slot 的 commitee 中 validator 的数量是均等的，最开始就平均分配好的
5. RANDAO 根据 validator 的 balance 权重选择 proposer，proposer 可能也是该 slot 的 committee 成员，这种情况发生的概率是 1/32
6. 每个 epoch 的第一个 slot 是 checkpoint，如果 epoch 空置，checkpoint 向下一个 epoch 顺延
7. 一个 checkpoint 经过两个 epochs 会被 finalized

### 2024.4.14

学习资料：

 - https://ethos.dev/beacon-chain

接上次内容，同样是讨论 The Beacon Chain。

对于 validators 的奖惩制度主要分为六类：

1. 见证人奖励（不展开）
2. 见证人惩罚
    > 不证实 block 或者证实的 block 没有被 finalized，产生处罚
3. 日常的不在线处罚
    > 是赢得奖励的 75% 程度的处罚。例如可以获得 10% 的 APY，表现十分差的（但诚实不作恶的） validator 最多获得 -7.5% APY 的处罚，一年有 10% 时间不在线的 validator 差不多有 -0.75% APY 的处罚。
4. 削减和举报者奖励
    > 连续 8192 个 epochs 不在线的 validator 会被削减 1/32(0.5E) 的 balance，注意这里是 balance 不是 staking。如果一批 validators 同时不在线惩罚将上升，公式是 `validator_balance*3*fraction_of_validators_slashed`,如果 1/3 的 validators 同时不在线，那么第三个参数就是 1/3，公式的结算结果将是 32E，那么这批同时下线的 validators 每个人的所有 balance(32E)就全部被罚没了。与此同时，举报他人不在线可以得到奖励。
5. 提案人奖励（不展开）
6. 不作为惩罚
    > 触发条件：当网络中无法达到足够的共识，即连续四个Epoch（大约51.2分钟）都没有区块达到终结性时，不活跃泄露机制会被触发。
    >
    > 惩罚方式：一旦触发，所有验证者开始遭受不活跃泄露惩罚，这种惩罚是以二次方方式增加的，也就是说惩罚随时间的增长而迅速增大。
    >
    > 目的：这种设计的目的是在网络中出现大规模不活跃或者故障时，通过削弱不活跃验证者的影响力，快速恢复网络的正常运作。当大量验证者不活跃时，这个机制通过逐渐减少他们的抵押份额，最终使得剩余活跃的验证者能够构成网络所需的2/3多数，恢复区块的终结性。
    >
    > 影响：在不活跃泄露期间，所有验证者的见证人（attester）奖励被设置为零，但是他们仍然可以获得提案者（proposer）和告密者（whistleblower）奖励。
    >
    > 示例效果：如果50%的验证者离线，根据不活跃泄露机制的设计，大约在18天后，网络将会自动恢复到能够再次达到区块终结性的状态，因为不活跃的验证者会被逐渐排除出去。

### 2024.4.15

学习资料：

 - https://epf.wiki/#/eps/week5
 - https://ethereum.org/en/roadmap/
 - https://domothy.com/roadmap/
 - https://ethereum.org/en/community/research/#active-areas-of-ethereum-research

#### Roadmap:

The Merge:

已经完成：

  - Beacon chain
  - PoW -> PoS
  - Staking 提现

计划中：

 - Secret leader election (SLE)
 - Single Slot Finality
 - Quantum-safe aggregation-friendly signatures

The Surge:

已经完成：

 - 4844 Proto-Danksharding

计划中：

 - Rollup scaling

### 2024.4.16

学习资料：

 - https://mp.weixin.qq.com/s/OQmXEPnk_kbrwaD_caT4sg
 - https://www.theblockbeats.info/news/51310
 - https://dune.com/hahahash/eigenlayer

14 日学习完 beacon chain 的运作之后，初步了解了节点的奖励和罚没机制，今日入门了一些关于 staking 和 restaking 生态的基础知识，一边学习一边参与了一个 eigenlayer 生态项目的 restaking，准备继续研读 eigenlayer 的合约代码以便深入学习。

### 2024.4.17

学习资料：

 - https://docs.eigenlayer.xyz/eigenlayer/avs-guides/incredible-squaring
 - https://eth2book.info/capella/part2/building_blocks/signatures/
 - https://github.com/Layr-Labs/incredible-squaring-avs

学习了 eigenlayer 的 AVS 相关的应用，以及合并签名相关的算法等。

### 2024.4.18

学习资料：

 - https://pendle.gitbook.io/pendle-academy/ecosystem-and-resources/points-trading/lrt-support-page
 - https://docs.pendle.finance/ProtocolMechanics/Glossary
 - https://app.pendle.finance/trade/education/learn?level=1
 - https://eips.ethereum.org/EIPS/eip-5115
 - https://research.mintventures.fund/2023/04/19/zh-pendle-unpacking-the-potential-of-the-crypto-interest-rate-market-with-lsd-market-service-provider/

学习了 pendle 协议 SY PT YT 的定义和应用，阅读了 EIP-5115 的解释，对整个 restaking 生态的上下游更了解了一些。

以DAI为例，当用户把100个DAI存入Pendle之后，Pendle会先把DAI存入compound中，变成100个cDAI。然后，Pendle会把100个cDAI封装成标准化收益代币sy-cDAI（Standardized Yield，缩写成sy），随后拆分成100个本金代币（principle token，缩写为PT）PT-cDAI和100个收益代币（yield token，缩写为YT）YT-cDAI。其中，每一个本金代币PT-cDAI在到期后，都可以兑换一个DAI；每一个YT-cDAI，可以兑换在持有期间的cDAI收益。

我们可以把本金代币PT看成是零息债券。越接近到期日，PT的价格就越接近面值；收益代币YT则获得持有期间的任何收益。比如，YT-cDAI在持有期间拥有借贷收益，也拥有Compound提供的COMP激励。

值得注意的是，在上述步骤中，YT+PT=SY。

以业务流程角度来看，用户在Pendle中存入资产后，一共会发生几个变化。我们在这里以用户存入的资产是ETH为例：

用户存入ETH，在“Zap”模式下，Pendle将自动通过Kyberswap把ETH换成stETH；
Pendle将stETH封装成sy-stTH。标准化收益代币（Standardized Yield，缩写成sy）是ERC-5115标准下的代币，该代币标准下可以封装绝大部分生息资产。该代币协议也是Pendle团队设计的代币标准；

如果用户选择“零价格冲击模式”（zero price impact mode），则Pendle会在第三步中，将一半的sy-stTH分拆成为收益代币YT-stETH和本金代币PT-stETH，并将PT-stETH与另一半sy-stTH组合成LP放入池子中，收益代币YT-stETH存放在用户账户中；如果用户不选择“零价格冲击模式”，则Pendle会在将PT-stETH与另一半sy-stTH组合成LP放入池子中的同时，把YT-stETH自动卖出，并将获得的资金购买更多PT-stETH。如果用户选择“manual”，则上述步骤都需要用户手动操作。

### 2024.4.21

学习资料：

 - https://domothy.com/blobspace/
 - https://www.youtube.com/watch?v=n4eiiCDhTes
 - https://foresightnews.pro/article/detail/17435
 - https://w3hitchhiker.mirror.xyz/iY0IB8yc9arir9yAEE-SmjgKdT7HP2CKsJOU5nS1Nc4

主要学习了关于 4844 blob 的存储方案和 specs，同时学习了 KZG commitment 在此处的应用。

关于擦出编码、采样数据可用性验证和 KZG 多项式承诺的介绍还是十分有趣的，特别是采样这一段：

> You pick a number i between 1 and 200,000 and ask the network for the evaluation of P(i). The probability that the i you picked happens to be one of the evaluations I did publish is at most 99,999/200,000≈1/2. You then repeat the process again. The probability that both succeeded is at most 1/4. You do it again. And again. After just 30 random checks, the probability that all of them just happened to be one of the minority data points I published is necessarily less than 1/(2^30)≈1/1,000,000,000. As you can see, it doesn’t take very long for me to fail this little game, since there’s no way I could have predicted ahead of time which random numbers you were going to ask for.

有了 KZG commitment，就可以在不访问原始 blob 数据的情况下，证明某个 L2 的 tx 属于这条 blob，这是普通的 hash 做不到的功能，对于 L2 数据验证也非常方便。

### 2024.4.23

学习资料：

 - https://blog.bingx.com/blockchain-en/what-are-sequencers-in-ethereum-network/

学习了关于 sequencer 的内容。A sequencer refers to a mechanism or component within a blockchain protocol that helps to establish the order of transactions or other operations.

Optimism 使用了中心化的 OP Labs 维护的 sequencer，如果该 sequencer 被恶意控制，那么整个 Optimism 将被控制。

Shared sequencer networks: 多个 rollups 可以共享的 sequencer（像是一种服务？）

Schnorr sequencer： 使用算法（看起来是一种多项式承诺）去确定某个 transaction 在特定的顺序上，而不用同时处理一批 transactions。这种模式是基于 mpc 的，每个 node 生成签名并通过 p2p 连接去广播，该模式适合 rollup 为自己建立的 sequencer 但不需要提交回 layer 1 的情况。


Espresso sequencer： 主要为 layer2 架构设计的 sequencer，提供高效率低延迟的 tx 排序服务，在 zk 和 op 模式下均可用。目前 Espresso sequencer 通过在 L1 上的 restaking 服务保障安全。

### 2024.4.24

学习资料：

 - https://epf.wiki/#/eps/week6-dev
 - https://paradigmxyz.github.io/reth/

学习了 reth 的基本设计思路和架构，尝试跑了 reth 客户端。

### 2024.4.25

学习资料：

 - https://epf.wiki/#/eps/week7-research
 - https://epf.wiki/#/wiki/EL/data-structures

复习了 MPT 相关的内容，学习了 Verkle Trees。同时对 Transaction Trie 结构有了了解，但是 World State Trie 还没有看特别明白， Receipt Trie 也尚且没有找到合适的学习资料，需要继续研究。关于 Verkle Tree 的细节还需要继续深入。

 > Verkle Trees
 >
 > Verkle tree is a new data structure that is being proposed to replace the current Merkle Patricia Trie. Named by combining the "Vector commitment" and "Merkle Tree", it is designed to be more efficient and scalable than the current MPT. It is a trie-based data structure that replaces the heavy witness used in the MPT with a lightweight witness. Verkle trees are the key part of The Verge upgrade of Ethereum Roadmap. They can enable stateless clients to be more efficient and scalable.
 >
 > Structure of Verkle Tree
 > 
 > The layout structure of a Verkle tree is just like a MPT but with different base of the tree i.e. number of children. Just like MPT it has root node, inner nodes, extension nodes and leaf nodes. There a slight difference in the key size, on which the tree is made. MPT uses 20 byte key which Verkle tree uses 32 byte key in which the 31 bytes are used as a stem of the tree while last 1 byte is used for storage with almost the same stem address or neighboring code chunks (opening the same commitment is cheaper). Also due to the fact that while computing the witness data the algorithms take 252 bit as field element so it is convenient to use 31 bytes as a suffix of the tree. Using this, the stem data can commit to two difference commitments ranging from 0-127 and 128-255, aka lower value and upper value of the same key, thus covering the whole suffix space. For more on this refer here.

### 2024.4.27

学习资料：

 - https://epf.wiki/#/eps/week8-dev
 - https://www.youtube.com/watch?v=6d4pkhL37Ao
 - https://www.youtube.com/watch?v=1PHZHpVPLk4
 - https://github.com/eth-protocol-fellows/protocol-studies/blob/main/docs/eps/presentations/week8-dev.pdf
 - https://eips.ethereum.org/EIPS/eip-7251

学习了 post-merge 时代 consensus client 的架构，为什么拆为 consensus 和 execution 两个客户端，以及了解了 Teku 客户端的基本架构设计。

Teku 在上层分为 execution-api/beacon-api/keymanager-api/builder-api 四个可操作部分，由 Java 语言编写，是目前多个 consensus client 其中之一。

同时学习饿了 EIP-7251 的内容，该提议将 `MAX_EFFECTIVE_BALANCE` 从 32E 提高到 2048E，这样有助于大型节点消减其 validator 数量，提高 BLS 签名的整体效率（更少的 validator 参与签名），降低网络的负载压力，同时还能提高资金使用的灵活度（小节点不需要拼 32 的整数倍，零散的 ETH 也能充分得到利用）。

### 2024.4.28

学习资料：

 - https://epf.wiki/#/eps/week8-research
 - https://github.com/eth-protocol-fellows/protocol-studies/blob/main/docs/eps/presentations/week8-research.pdf
 - https://barnabe.substack.com/p/seeing-like-a-protocol
 - https://ethereum.org/en/developers/docs/mev/
 - https://efdn.notion.site/Robust-Incentives-Group-RIG-Homepage-802339956f2745a5964d8461c5ccef02
 - https://github.com/eth-protocol-fellows/protocol-studies/tree/pbs/docs/wiki/research/PBS

Ethereum protocol 被社区驱动，以质押量作为话语权系数。社区决定了 protocol 设计，从而影响 validators 的行为，因为 validators 是最终 protocol 的执行者。Protocol 的最终目的是用最小的成本为用户去中心化地实现更大的利益。

Validator services 包括 Consensus service/Block construction service 两部分。（未完）

### 2024.4.29

学习资料：

 - https://ethresear.ch/t/unbundling-staking-towards-rainbow-staking/18683
 - https://efdn.notion.site/RIG-Open-Problems-ROPs-c11382c213f949a4b89927ef4e962adf
 - https://barnabe.substack.com/p/pbs




### 待学习

 - https://members.delphidigital.io/reports/the-hitchhikers-guide-to-ethereum
 - https://www.youtube.com/watch?v=UClaoL12W00
 