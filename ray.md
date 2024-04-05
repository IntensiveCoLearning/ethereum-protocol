# [your name]
Hi, I am ray, I know a bit about ethereum, and I am trying to become an expert in it.

- Twitter: https://twitter.com/rayjun0412
- tg: https://t.me/rayjun0412

## Notes

### 2024.4.3

Trying to understand POS more deeply

### 2024.4.5

*以太坊的 PoS 是什么，是如何工作的*

以太坊通过 The Merge 升级，将共识机制从工作量证明（PoW）转成了权益证明（PoS）。 如果想参与到以太坊的网络，需要质押 32 ETH（这个数额有可能会调整）成为 Validator。

Validator 负责处理交易、创建新的区块，验证现有的区块，以太坊的 PoS 协议是 Gasper，来自 Casper （权益证明算法）和LMD GHOST（分叉选择算法）的结合。

如果想要成为网络的 Validator，需要先到一个智能合约中质押 ETH，然后可以参与到网络新区块的创建、区块的验证，并在其中留下自己的签名，其他人从签名中获取地址，通过这个智能合约中的来检验 Validator 的身份，*Validator 的签名就是 Proof of Stake*。

如果 Validator 作恶，那么罚没质押的资金（不同的作恶情节有不同额度的罚没），如果被罚没之后资金量少于 16 ETH，那么就会被提出网络。。

区块链的共识其实就是让链的高度可以持续增加。有两种方式：
- 最长链模型
- BFT

而以太坊的 Casper 协议结合了这两种方式，在后续，将尝试用最简单的语言将 PoS 的机制解释清楚。

PoS 比 PoW 更节省能源，也更适合以太坊长远的规划。但是我觉得 PoS 算法太复杂了，没有 PoW 那么优雅，这是一个技术上的妥协，而且我认为 PoS 最大缺陷在于 permissionless 上比 PoW 要差，如果是 PoW 机制，任何人可以在任意时间加入网络开始挖矿，但是对于 PoS 来说，需要先质押 ETH， 然后才能参与网络，而质押 ETH 是一笔交易，需要先完成执行，然后上链。所以如果有很多人参与网络或者退出网络，通常需要有一个排队的过程。
