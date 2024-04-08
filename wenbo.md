# [Wenbo]

[Hello，my name is Wenbo and I'm a undergraduate student who loves BlockChain and Deeplearning. I'm looking forward to learning about the Ethereum Protocol by attending intensive-study-group.ヾ(≧▽≦*)o]

## Notes
### 2024.4.8
1. 快速过了：inevitableeth.com/en/home/background 和 inevitableeth.com/en/home/background/mass-comm, ![y](印刷技术传播图tps://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/78262508/4369768c-2735-4d2c-a6ce-12f205cad48c) 让我想起之前的分析说奥斯曼帝国对印刷术的各种严厉限制措施是中亚识字率甚至低于大清的主要原因。
2. 看了：inevitableeth.com/home/ethereum/upgrades/scaling/data
> Our primary goal: credible neutrality through decentralization. If we lose $ETH decentralization, we lose everything.

> PBS gives us the ability to propose our blobs, but we still need to address our biggest problem: how can we achieve 100% data availability without forcing any nodes to download 100% of the data?PBS 使我们能够提出 blob，但我们仍然需要解决最大的问题：如何在不强制任何节点下载 100% 数据的情况下实现 100% 数据可用性？
Well, we'll just distribute it across the P2P network!好吧，我们将通过 P2P 网络分发它！
Here's what's important: each node will download just a small data sample from each blob. No single node will be required hold an entire blob, just tiny fractions. These tiny fractions will be efficiently distributed across the network to ensure that it is always available.重要的是：每个节点只会从每个 blob 下载一个小数据样本。不需要单个节点保存整个 blob，只需要保存很小的部分。这些微小的部分将有效地分布在网络上，以确保它始终可用。
Upon request, the network will be able to quickly/efficiently reconstruct a blob. 根据请求，网络将能够快速/高效地重建 blob。

BT用户就很熟悉了。。。
想起来之前看的 telegra.ph/白话解读区块链不可能三角的变革性解决方案-03-17，感觉讲的更深入浅出一点，但现在结合来看又有新收获


### 2024.4.6
1. As hinted above, the main high level components of Ethereum are execution and consensus layer. These are 2 networks which are connected and dependent on each other.
如上所述，以太坊的主要高级组件是执行层和共识层。这是两个相互连接且相互依赖的网络。
2. Execution layer provides the execution engine, handles user transaction and all state (address, contract data) while consensus implements the proof-of-stake mechanism ensuring security and fault tolerance.
执行层提供执行引擎，处理用户交易和所有状态（地址、合约数据），而共识则实现权益证明机制，确保安全性和容错性。
3. The coordination mainly happens via regular calls which are scheduled in the PM repo. There are different kinds of developer calls with the biggest one being All Core Devs (ACD). This is where representatives of all involved teams come to discuss the current development of the consensus or execution layer.协调主要通过 PM 存储库中安排的定期调用进行。开发人员电话会议有多种类型，其中最大的一种是所有核心开发人员电话会议 (ACD)。所有相关团队的代表都在这里讨论共识层或执行层的当前发展。
4. Tried https://ethereum.org/zh/quizzes/, interesting :)

### 2024.4.5

> The Free Software movement is fundamental to Ethereum and all cryptocurrencies. The open, independent and collaborative development culture of Ethereum is strongly rooted in FOSS (Free and Open Source Software). Ethereum needs to be transparently implemented in software that gives full freedom to its users.

~~Pending....~~ Busy🥲

### 2024.4.4

Start
