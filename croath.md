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
