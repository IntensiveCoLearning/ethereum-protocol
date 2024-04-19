# Derick

hi guys, my name is Derick and I'm a back-end programmer who loves technology. I'm looking forward to learning about the Ethereum Protocol by attending https://epf.wiki/

## Notes

### 2024.4.19

#### 了解以太坊的EL Client Reth

##### What are the goals of Reth?

1. Modularity

- Every component of Reth is built to be used as a library: well-tested, heavily documented and benchmarked. We envision that developers will import the node's crates, mix and match, and innovate on top of them.

- Examples of such usage include, but are not limited to, spinning up standalone P2P networks, talking directly to a node's database, or "unbundling" the node into the components you need.

- To achieve that, we are licensing Reth under the Apache/MIT permissive license.

2. Performance

- Reth aims to be fast, so we used Rust and the Erigon staged-sync node architecture.

- We also use our Ethereum libraries (including Alloy and revm) which we’ve battle-tested and optimized via Foundry.

3. Free for anyone to use any way they want

- Reth is free open source software, built for the community, by the community.

- By licensing the software under the Apache/MIT license, we want developers to use it without being bound by business licenses, or having to think about the implications of GPL-like licenses.

4. Client Diversity

- The Ethereum protocol becomes more antifragile when no node implementation dominates. This ensures that if there's a software bug, the network does not finalize a bad block. By building a new client, we hope to contribute to Ethereum's antifragility.

5. Used by a wide demographic

- We want to solve for node operators that care about fast historical queries, but also for hobbyists who cannot operate on large hardware.

- We also want to support teams and individuals who want both sync from genesis and via "fast sync".

- We envision that Reth will be configurable enough for the tradeoffs that each team faces.


### 2024.4.18

#### 数据分片和DAS提案解释
今天学习了v神发布的一篇[文章](https://hackmd.io/@vbuterin/sharding_proposal)，是一份提案，该提案使用侧重数据可用性的方法来构建以太坊分片实现的第 1 阶段。

1. 信标链的主要新增内容将是一个 ShardDataHeader 对象的向量,每个分片一个。ShardDataHeader 表示大量的底层数据(大小为 0-512 kB)。

2. 只有当 ShardDataHeader 所指向的底层数据可用时,区块才有效,这意味着它已经发布到网络上,任何人都可以下载。

3. 为了保持可扩展性,客户端在验证区块时不会下载每个 ShardDataHeader 的完整底层数据。相反,他们将使用一种称为数据可用性抽样的间接技术来验证数据可用性。

4. 文章还讨论了与分片实现相关的一些参数,例如分片数量、每个 epoch 的槽位数以及每个委员会周期的槽位数。

提出了一种侧重数据可用性的方法,用于以太坊分片实现的第 1 阶段,引入了 ShardDataHeader 对象和数据可用性抽样,以确保数据可用性,同时保持可扩展性。



### 2024.4.17

#### 以太坊共识层和执行层规范

- 通过阅读week6中的资料 
1. https://github.com/ethereum/consensus-specs 
2. https://blog.ethereum.org/2023/08/29/eel-spec

共识层规范(Consensus Layer Spec):
- 以太坊共识层负责维护以太坊区块链的安全性和一致性,目前采用的是权益证明(PoS)共识机制。
- 在PoS下,验证者需要质押ETH作为抵押,并参与区块提议和投票过程。验证者的权重与其质押的ETH数量成正比。
- 为了防止验证者作恶,以太坊引入了罚没机制。如果验证者违反协议,其质押的部分ETH将被罚没。
- 共识层的主要组件包括信标链(Beacon Chain)、分片(Sharding)等。信标链是PoS的核心,负责管理验证者集合和协调分片。
- 未来以太坊计划实现Danksharding,进一步提高可扩展性。Danksharding允许验证者只保存和验证分片数据的一部分。

执行层规范(Execution Layer Spec):
- 以太坊执行层负责处理交易和维护状态,目前采用的是以太坊虚拟机(EVM)。
- EVM是一个图灵完备的虚拟机,支持通过智能合约执行任意复杂度的计算。
- 执行层的关键组件包括交易池(Txpool)、状态存储(State)、收据存储(Receipt)等。
- 为了提高执行层的可读性和可维护性,以太坊社区推出了以太坊执行层规范(EELS)。
- EELS是用Python编写的执行层参考实现,为每个分叉提供了完整的协议快照,有助于开发者理解和实现EIP。
- EELS的目标是成为指定核心EIP的默认方式,也是EIP作者首先用来原型开发提案的工具。



### 2024.4.16

#### 以太坊节点架构

- 通过阅读  https://ethereum.org/en/developers/docs/nodes-and-clients/node-architecture/ 了解了以太坊节点架构

- 以太坊节点由两个客户端组成:执行客户端和共识客户端。

- 在以太坊使用工作量证明时,只需要执行客户端就可以运行一个完整的以太坊节点。但自从实施权益证明后,执行客户端需要与另一个称为"共识客户端"的软件一起使用。

- 两个客户端分别连接到各自的对等网络(P2P)。执行客户端通过P2P网络传播交易,管理本地交易池,而共识客户端通过P2P网络传播区块,实现共识和区块链增长。

执行客户端的作用包括:
- 作为以太坊的用户网关 
- 包含以太坊虚拟机、以太坊状态和交易池
- 提供JSON-RPC接口供用户查询区块链、提交交易和部署智能合约

- 共识客户端的作用是参与权益证明共识,验证执行客户端的计算结果。节点运营者可以通过在质押合约中存入32 ETH来给共识客户端添加验证者角色。

- 新的以太坊节点架构采用模块化设计,由执行客户端和共识客户端组成,分工明确,共同维护以太坊网络的安全和去中心化。

### 2024.4.15

#### 以太坊存储新解决方案探索

- 自EIP-4844之后，以太坊网络的数据吞吐量与存储压力日益增长，不断增长的存储需求为以太坊节点带来了巨大挑战。为了降低存储压力，部分以太坊客户端对本地存放的以太坊历史数据进行删剪，不同的全节点在存储行为上的一致性被逐渐瓦解。
-  EIP-4444：限制执行客户端中的历史数据：该提案允许客户端删除超过一年的过往区块。假设平均区块大小为 100K，历史块数据上限约为 250 GB（100K * (3600 * 24 * 365) / 12，假设区块时间 = 12 秒）。

- EIP-4844：分片 BLOB 交易：丢弃超过 18 天的 BLOB数据。与 EIP-4444 相比，这是一种更激进的方法，将历史 BLOB 大小限制在 100 GB 左右（(18 * 3600 * 24) * 128K * 6 / 12，假设区块时间 = 12 秒）。
- 昨天做了执行客户端端的介绍，其中Geth是保留全部数据，并且不提供api进行删减历史数据，而其他客户端如Nethermind 和 Besu 以配置删除某些以太坊历史数据（例如历史区块）。这会让部分客户端节点的行为与其他客户端不一致。

##### 删除所有客户端的历史数据会产生什么后果？
- 新节点无法通过“full sync”模式来同步到最新状态， “full sync”是一种将历史数据重放，从创世区块同步到最新区块的数据同步方案。相应地，我们必须采取“snap sync”或“state sync”来直接同步以太坊节点的最新状态。这种方法已在 Geth 中实现，并作为默认的同步运行方式。
- 节点删除掉以太坊主网历史数据，也会导致以太坊 L2出现问题，即新加入的Layer2节点，无法通过重放 Layer2全部历史数据的方法，同步至当前的最新状态。此外，由于 L1 节点不维护 L2 状态，L2 的“snap sync”方法无法根据Layer1区块直接派生出最新的 Layer2 状态，这违反了Layer2继承以太坊安全所需的重要假设。

##### 可参考的解决方案

###### 方案1：以太坊 Portal 网络
- 以太坊 Portal 网络是一个轻量级、去中心化的网络，用于连接到以太坊协议。它提供eth_call，eth_getBlockByNumber等以太坊 JSON-RPC 接口，它将 JSON-RPC 请求转换为对分布式哈希表（DHT)的 P2P 请求，类似于 IPFS 网络。与允许存储任何数据类型且容易受到垃圾数据影响的 IPFS 不同，Portal P2P 网络专门托管以太坊数据，如历史区块头和交易数据，这是通过 Portal 网络内置的轻客户端验证技术来实现的。

- Portal 网络的一个重要特性是。其轻量级的运行设计以及与资源受限设备的兼容性。它可以运行在具有几MB存储空间和低内存的节点之上，从而促进去中心化。即使是手机或 Raspberry Pi 设备也有可能加入该网络，为解决以太坊DA问题做出贡献。

###### 方案2：EthStorage 网络

- EthStorage 网络是一个去中心化的激励存储网络，专门用于存储 EIP-4844 BLOB，并获得 ESP 项目的资助。

- 最小信任：与需要中心化数据桥的现有解决方案不同，EthStorage 依赖于以太坊的共识和无需许可的 EthStorage 存储节点的 1/m 信任模型。存储 BLOB 的过程是这样的：用户签署一个携带 BLOB 的交易，调用存储合约的put(key, blob_idx) 方法。然后存储合约将记录 BLOB 哈希在链上。之后存储提供商将直接从以太坊 DA 网络下载并存储 BLOB，从而绕过数据桥问题。

- 存储成本与激励相一致：当调用 put() 方法时，交易必须发送存储费（通过 msg.value）并存入合约中。在成功链下存储节点提交并验证存储证明后，这个存储费用将随着时间的推移逐渐分配给存储节点。与现有的向出块者(proposer)支付一次性存储费的以太坊存储费模型相比，随着时间的推移，支付的存储费遵循贴现现金流模型——假设随着时间的推移，存储成本将相对于 ETH价格而降低。EthStorage 引入的这一重大创新使得费用和存储节点的存储贡献保持一致。

- 存储证明：存储证明是受到数据可用性抽样的启发，而 EthStorage 中的采样是针对一段时间内的保存的BLOB。为了有效地验证链上采样，EthStorage 充分利用了智能合约和最新的 SNARK 技术发展。

- 无许可操作：EthStorage 中的任何存储节点只要存储数据并定期在链上提交存储证明，都可以获得报酬。



### 2024.4.14

- 了解了共识客户端和执行客户端信息


#### Consensus Clients

| Client    | 占比| Status | Support                | Language   |
|-----------|-  |--------|------------------------|------------|
| Grandine  |   | beta   | Linux, Win, macOS      | -          |
| Lighthouse| 33.47%  | stable | Linux, Win, macOS, ARM | Rust       |
| Lodestar  | 0.92%   | stable | Linux, Win, macOS      | TypeScript |
| Nimbus    |9.34%   | stable | Linux, Win, macOS, ARM | Nim        |
| Prysm     |37.89%   | stable | Linux, Win, macOS, ARM | Golang     |
| Teku      |18.39%   | stable | Linux, Win, macOS      | Java       |

Data provided by Sigma Prime's Blockprint — updated daily.
Data may not be 100% accurate. 


#### Execution Clients

| Client     | 占比|Status       | Support                | Language   |
|------------|-|-------------|------------------------|------------|
| Akula      | |deprecated   | -                      | -          |
| Besu       |12% |stable       | Linux, Win, macOS      | Java       |
| Erigon     |2% |alpha & beta | Linux, Win, macOS, ARM | Golang     |
| EthereumJS | |alpha        | Linux, Win, macOS      | TypeScript |
| Geth       |63% |stable       | Linux, Win, macOS, ARM | Golang     |
| Nethermind |23% |stable       | Linux, Win, macOS, ARM | .NET       |
| Nimbus     | |pre-alpha    | -                      | Nim        |
| Reth       | |alpha        | Linux, Win, macOS, ARM | Rust       |
| Silkworm   | |pre-alpha    | Linux, Win, macOS      | C++        |

Data provided by supermajority.info — updated manually.
Data may not be 100% accurate. 
##### Client Diversity Is Not Optional

> 许多人知道客户端的多样性对于构建一个更有韧性的网络非常重要，但他们不明白为什么这么重要，或者它的重要性到底有多大。这不仅仅是重要——它是至关重要的。如果有一个单一的客户端被三分之二（66%）的验证者使用，这将非常真实地带来中断链条和节点操作者经济损失的风险。
达到最终一致性需要三分之二的验证者。如果一个拥有超过66%市场份额的客户端出现了一个错误，并分叉到它自己的链上，它将能够完成最终确定。一旦分叉完成最终确定，验证者就无法返回到真正的链上，而不被惩罚。如果66%的链同时被惩罚，那么罚款就是全部的32 ETH。
那么，为什么超过50%的市场份额仍然危险呢？如果一个少数客户端分叉，那么超过50%的多数客户端可以获得超过66%的多数。如果没有任何一个客户端的市场份额超过33%，这些情况就可以避免。这就是为什么所有客户端的目标市场份额是小于33%。
执行客户端也不是免疫的。上面提到的风险同样适用于共识客户端和执行客户端。

### 2024.4.13

#### zk/op rollup
ZK Rollups和Optimistic Rollups是以太坊上两种主要的二层扩容解决方案，它们都通过将交易计算和存储转移到链下，同时依赖以太坊主链的安全性来显著提高交易吞吐量和降低成本。下面详细解释它们的原理和区别:

##### ZK Rollups 

ZK Rollups利用零知识证明技术，在链下执行交易并生成加密证明，证明这些交易的正确性和合法性，然后将证明提交到主链进行验证。主要特点包括:

- 通过压缩交易数据和使用通用或专用电路生成证明，显著减少在主链上的数据量
- 主链只需验证证明的合法性，不需要重放所有交易，因此验证成本相对较低(每个批次约50万gas)
- 提取资金非常快，只需等待下一个批次的交易
- 零知识证明技术相对复杂，通用计算电路的证明生成成本很高
- 代表项目有zkSync、Loopring、Starkware等

##### Optimistic Rollups 

Optimistic Rollups采用欺诈证明，乐观地假设链下的交易都是正确的，除非有人提出质疑并提供欺诈证明。主要特点包括:

- 将批量交易数据作为简单的状态根变更提交到主链，每个批次的固定成本只有4万gas
- 如果有错误交易，需要在挑战期内提交欺诈证明，因此提款需要等待1周左右
- 技术门槛相对较低，更容易实现EVM兼容的通用计算
- 每笔交易在链上的成本相对ZK Rollup略高，因为需要将完整交易数据发布上链
- 代表项目有Optimism、Arbitrum等

 https://www.aicoin.com/article/324592.html


### 2024.4.12

#### EIP-4844

EIP-4844 被称为 proto-danksharding，它是以太坊的 Dencun 硬分叉中包含的一组升级提案之一。


##### EIP-4844 的技术细节

* 引入「blob」数据块 本质上是大数据包，可以比当前以太坊使用的 calldata 更有效地处理和存储数据。这些 blob 可以存储大量信息，例如去中心化交易所的订单簿数据。

* 区块链验证的权衡：验证器节点（以前称为矿工）仍然需要完全验证交易本身，但他们可以選擇不下载和验证相关联的 blob 数据。这降低了运行以太坊节点所需的计算量和存储空间。
  
* Rollup 是以太坊扩展解决方案的重要组成部分，它们将交易数据存储在链下，以降低成本并提高吞吐量。EIP-4844 将允许 rollup 使用这些 blob 来存储交易数据，而无需将其包含在主网上需要支付 gas 费用的每一笔交易中。
  
* 数据有效期有限：为了平衡去中心化和可扩展性，EIP-4844 规定了 blob 数据的有效期。这些数据块仅在共识层上存储有限的时间（通常约 18 天），然后将被删除。

##### EIP-4844 如何降低 `gas` 费用？

它提出了一种新的交易类型，称为「blob携带交易 (`blob-carrying transactions`)」。这种交易类似于附加在以太坊交易上的「sidecar」，可以包含额外的信息块。这些 blob 的体积很大（最大可达 128 KB），但与当前的 calldata 相比费用更低。通过将这些 blob 临时存储在以太坊的共识层上，EIP-4844 旨在显着降低 rollup 向以太坊主网传输数据的成本，从而实现降低交易费用。


##### EIP-4844 的优点

* 降低交易费用：EIP-4844 的主要优势之一是降低交易费用。通过将交易数据和执行分开，EIP-4844 使得 rollup 等第 2 层扩展解决方案可以更便宜地存储数据。
* 为分片扩展奠定基础：EIP-4844 的设计与以太坊未来的分片扩展路线图兼容。分片是一种将区块链数据分布到多个分片中的方法，可以显着提高可扩展性。EIP-4844 引入了分片所需的一些技术要素，例如 blob 和分离的交易数据。

##### EIP-4844 的潜在影响

EIP-4844 预计将对以太坊生态系统产生重大影响。它可以降低交易费用，使以太坊对更广泛的用户和用例更具吸引力。它还可以通过为分片扩展奠定基础，为以太坊的可扩展性带来长期收益。

EIP-4844 的重要之处在于它可以在不增加去中心化性的情况下提高可扩展性。虽然共识节点需要下载这些 blob，但由于它们仅保存有限的时间，因此不会对长期存储需求造成太大压力。

EIP-4844 将为以太坊带来以下好处：

* 降低 Layer 2 rollup 解决方案的交易费用
* 为以太坊未来更复杂的扩容方案（例如完整分片）奠定基础

### 2024.4.11

#### ethereum/execution-spec-tests

`ethereum/execution-spec-tests`框架收集并执行测试用例，以生成测试夹具（JSON），任何执行客户端都可以使用该夹具来验证其以太坊/执行规范的实现。fixture定义了状态转换和块测试，由框架使用大多数执行客户端提供的 `t8n` 命令行工具之一来生成.

| Client | t8n Tool | Tracing Support |
|--------|----------|-----------------|
| ethereum/evmone | evmone-t8n | Yes |
| ethereum/execution-specs | ethereum-spec-evm | Yes |
| ethereum/go-ethereum | evm t8n | Yes |
| hyperledger/besu | evm t8n-server | No |
| status-im/nimbus-eth1 | t8n | Yes |

##### 配置测试环境
```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum solc
```

```
git clone https://github.com/ethereum/execution-spec-tests
cd execution-spec-tests
python3 -m venv ./venv/
source ./venv/bin/activate
pip install -e '.[docs,lint,test]'
```

##### 验证测试环境
```
fill --collect-only
fill -v tests/berlin/eip2930_access_list/test_acl.py
```
##### 增加新测试用例
```
📁 execution-test-specs/
├─╴📁 tests/
|   ├── 📄 __init__.py
│   ├── 📁 cancun/
|   |    ├── 📄 __init__.py
│   |    └── 📁 eip4844_blobs/
|   |        ├── 📄 __init__.py
|   |        ├── 📄 test_blobhash_opcode.py
|   |        ├── 📄 test_excess_blob_gas.py
|   |        └── 📄 ...
|   ├── 📁 shanghai
|   |    ├── 📁 eip3651_warm_coinbase
|   |    |   ├── 📄 __init__.py
|   |    |   └── 📄 test_warm_coinbase.py
|   |    ├── 📁 eip3855_push0
|   |    |   ├── 📄 __init__.py
|   |    |   └── 📄 test_push0.py
|   |    ├── 📁...
|   |    ...
│   └── 📁 ...
```
每个测试用例都被定义为一个Python函数，该函数通过使用框架提供的 state_test 或 blockchain_test 对象之一来定义单个 StateTest 或 BlockchainTest 。测试用例和测试模块必须满足以下要求：

## Ethereum Test Case Requirements

| Requirement | When |
|-------------|------|
| Be decorated with validity markers | If the test case is not valid for all forks |
| Use one of `state_test` or `blockchain_test` in its function arguments | Always |
| Call the `state_test` or `blockchain_test` in its test body | Always |
| Add a reference version of the EIP spec under test | Test path contains `eip` |

对于状态测试：
`def test_access_list(state_test: StateTestFiller):`

对于区块链测试：
```
def test_contract_creating_tx(
    blockchain_test: BlockchainTestFiller, fork: Fork, initcode: Initcode
):
```

`state_test` 和 `blockchain_test` 对象实际上分别是 `StateTest` 和 `BlockchainTest` 对象的包装器类，它们一旦被调用就实际上实例化这些对象的新实例，并根据测试中定义的前状态和后状态以及事务使用 evm 工具填充测试用例。

### 2024.4.10

#### ethereum测试

Retesteth是以太坊测试工具，用于对以太坊客户端进行共识测试、区块测试、状态测试等。工作流程如下：

1. 准备测试用例。Retesteth会读取定义好的测试用例文件，这些文件描述了具体的测试场景，包括预置状态、执行的交易、预期结果等。测试用例存放在tests仓库中。

2. 配置被测客户端。在运行测试前，需要先设置好待测试的以太坊客户端，如geth、besu、nimbus等。可以通过环境变量、命令行参数等方式传递客户端信息给Retesteth。

3. 执行测试。通过Retesteth命令行启动测试，它会逐个读取测试用例文件，然后调用以太坊客户端执行相应的测试场景。在测试过程中，Retesteth会:
   - 将测试用例的预置状态导入到客户端
   - 发送测试交易给客户端执行
   - 获取客户端执行完交易后的状态
   - 将实际状态与预期结果进行比对，判断测试是否通过

4. 生成测试报告。测试完成后，Retesteth会生成测试报告，展示测试通过和失败的情况。对于未通过的测试用例，可以分析具体失败的原因，并反馈给客户端开发者进行修复。

5. 补充测试用例。如果在测试中发现了客户端实现的问题，可以据此补充新的测试用例到tests仓库中，丰富测试内容。

Retesteth作为以太坊规范的参考测试工具，通过运行各类测试场景，验证不同以太坊客户端的一致性，是保障以太坊生态健康发展的重要手段。


> Guillaume Ballet的讲座和演示集中讨论了Verkle树带来的技术变化，它们的实现现状，以及无状态以太坊的愿景。开发工作涉及大量关于密码学、数据结构和共识机制的工作，以将Verkle树整合到以太坊中。
#### Verkle树简介

Verkle树是一种数据结构，代表了从当前以太坊使用的Merkle Patricia树（MPT）的演进。它们结合了向量承诺，允许更高效的数据存储和检索。这种效率对于以太坊的可扩展性和性能改进至关重要。

##### Verkle树的好处

1. **更小的证明大小**：Verkle树显著减少了验证区块链状态所需的证明（见证）的大小。证明大小的减少意味着证明可以更高效地在网络上传递，使新类型的功能成为可能，并使无状态客户端可行。

2. **更低的硬件要求**：通过使以太坊状态更加紧凑和证明更小，Verkle树降低了运行以太坊节点的硬件要求。这种改进使得更多设备，甚至可能是智能手机，能够参与网络，从而增强了去中心化。

3. **新节点更快的同步**：Verkle树的效率允许新节点加入网络并更快地同步，比当前的MPT更快。这种更快的同步提高了以太坊网络的整体鲁棒性和可访问性。

4. **可能的更高gas limits**：减少的计算开销和提高的效率可能允许更高的gas limits，使以太坊能够每秒处理更多交易，提高其可扩展性。

5. **与未来创新的兼容性**：Verkle树与未来以太坊创新，如zk-EVMs（零知识以太坊虚拟机）更加兼容。这种兼容性对于以太坊的长期发展及其能够整合尖端加密技术至关重要。



### 2024.4.9

#### 关于POS的讨论

地址：https://domothy.com/proof-of-stake/

这篇文章主要讨论了以太坊从工作量证明(PoW)转向权益证明(PoS)共识机制的话题,包括:

1. PoS并非一开始就被以太坊采用,主要原因是早期PoS还处于理论研究阶段,存在一些基本问题有待克服,且不同区块链的PoS实现各有利弊,没有放之四海而皆准的方案。

2. 有观点认为PoS会导致中心化,本质上与PoW类似,拥有更多资金的人可以获得更高的收益。但在PoS下,不管质押10美元还是1000万美元,每个人的收益率是一样的。

3. 虽然没有像PoW那样重复计算哈希,但验证者在PoS下仍然要做"工作",即创建和验证区块,只是这种工作完全由区块链达成共识所需的有用工作组成。

4. 文章认为,无论是PoW还是PoS,拥有资金优势的人都能获得更多的算力或质押币,从而获利更多。PoW因规模效应会导致矿池中心化,而PoS下大户的收益率与小户一致。

这篇文章分析了PoS相对PoW的优缺点,指出二者在趋于中心化方面并无本质区别。PoS的核心在于让验证者做有助于区块链共识的有意义工作,而非单纯计算哈希。以太坊选择PoS是经过长期研究和权衡的结果。


### 权益证明（PoS）介绍

https://consensys.io/blog/what-is-proof-of-stake

权益证明（Proof of Stake, PoS）是一种加密货币共识机制，用于验证和添加新的交易记录到区块链。与工作量证明（Proof of Work, PoW）相比，PoS不依赖于矿工通过解决复杂计算问题来竞争新区块的创建权，而是基于参与者持有的加密货币数量和持有时间来选择验证者。

### PoS的优势

1. **能源效率**：PoS机制不需要大量的计算资源，因此相比PoW，它大大减少了能源消耗。
2. **降低参与门槛**：由于不需要昂贵的计算硬件，更多的用户可以参与到验证和维护网络的过程中。
3. **加强网络安全**：在PoS中，验证者需要质押（锁定）一定数量的加密货币作为参与验证的“抵押”。如果验证者试图攻击网络或验证不正确的交易，他们将有可能失去部分或全部的质押资金。
4. **更公平的奖励分配**：所有验证者根据他们质押的资金比例获得奖励，这意味着收益率对于大户和小户是一致的。

### 以太坊2.0和PoS

以太坊2.0是以太坊网络的一次重大升级，其核心是从PoW转向PoS共识机制。这一转变旨在解决以太坊当前面临的扩展性、能源效率和安全性问题。

1. **更高的交易吞吐量**：通过引入分片技术和PoS，以太坊2.0旨在大幅提高网络的处理能力，使其能够处理更多的交易和应用。
2. **更低的参与门槛**：与PoW相比，PoS允许更多的用户以较低的成本参与到网络的维护中，增加了网络的去中心化程度和安全性。
3. **环境友好**：PoS显著减少了能源消耗，与日益增长的环保意识相符。

权益证明机制为加密货币网络提供了一种更加高效、安全和环保的共识机制。以太坊2.0的升级标志着对这一机制的重大采纳，预计将为整个区块链行业带来深远的影响。

- Proof of Stake addresses the three issues of PoW chains discussed earlier - accessibility, centralization, and especially scalability:

#### 如何在POS中处理网络中的故障或恶意节点 

在权益证明(PoS)共识机制中,分布式网络通过拜占庭容错(BFT)算法来处理故障或恶意节点的问题,以确保网络的正常运行。具体来说:

1. PoS网络利用BFT算法在存在一定数量的故障或恶意节点的情况下仍能达成共识,只要故障节点的数量不超过一定阈值。

2. 验证者需要质押(锁定)一定数量的加密货币作为抵押。如果验证者试图攻击网络或验证错误的交易,他们将损失部分或全部的质押资金作为惩罚。这种惩罚机制激励验证者诚实行事。

3. 一些PoS区块链会对验证者的停机时间进行严厉惩罚,如果他们的在线时间低于一定阈值,就会销毁("削减")部分质押的币。这促使验证者保持高水平的可用性。

4. PoS共识的安全性直接掌握在那些维护网络并在协议中持有原生加密资产的人手中。与PoW相比,这种架构能更好地将网络安全与参与者的经济利益绑定在一起。

5. 尽管PoS在安全性方面可能不如PoW得到充分验证,但通过质押、惩罚和激励等机制,PoS网络能有效应对故障和恶意行为,维护系统的可靠运行。

PoS网络主要通过BFT算法、质押和惩罚机制来处理故障或恶意节点。通过将验证者的经济利益与诚实行为绑定,并对不当行为进行严厉惩罚,PoS激励验证者维护网络的安全和可用性,从而确保系统在存在一定比例的拜占庭节点时仍能正常运行。



### 2024.4.8

#### 信标链(Beacon Chain)

信标链是以太坊2.0 PoS共识系统的核心协调机制。学习总结下它的工作原理:

1. 信标链是一个独立的区块链，与以太坊主链并行运行。它负责管理PoS协议并协调验证者网络。

2. 验证者持有32个ETH来参与共识过程。它们被信标链随机分配到每个槽位(12秒)至少128个验证者的“委员会”中，以提出和验证新区块。

3. 信标链将时间组织为“插槽”(12秒)和“epoch”(32个插槽，约6.4分钟)。每个槽位都有一个由信标链选择的区块提议者来创建一个新的区块。

4. 每个槽位委员会中的验证者通过广播“证明”对提议的区块进行投票。当委员会的绝对多数成员证明了一个区块时，它就会被添加到信标链。

5. 在每个epoch的开始，信标链从前一个epoch指定一个检查点块。如果代表总以太坊2/3的验证者投票支持该检查点，它就变得“合理”。如果下一个epoch的检查点也被证明是合理的，那么前一个检查点就会“最终确定”，这意味着它是不可逆的。

6. 信标链根据验证者的表现对其进行奖励和惩罚，并通过“砍”来惩罚恶意行为，导致验证者失去一些押注的ETH。

 https://consensys.io/blog/the-ethereum-2-0-beacon-chain-is-here-now-what



### 2024.4.7

#### 节点和客户端

在以太坊网络中，节点可以是轻节点、全节点或存档节点。轻节点只下载区块头，而不是整个区块链的所有数据，这使得它们可以快速同步并验证数据的有效性。全节点则存储整个区块链的数据，并参与到区块的验证过程中。存档节点存储了全节点的所有信息，并且还建立了历史状态的存档。

以太坊客户端的功能包括创建和管理钱包、发送交易、查询区块链信息、与智能合约交互以及管理本地节点等。

以太坊节点和客户端的设计旨在支持去中心化，确保网络的安全性和可靠性。每个节点都运行着以太坊虚拟机（EVM）并执行相同的指令，这些节点彼此平等，实时沟通同步区块资料，以维持以太坊区块链的运作。


[1] https://ethereum.org/en/developers/docs/nodes-and-clients/

[2] https://cs251.stanford.edu/lectures/lecture7.pdf


#### Execution Layer node

以太坊执行层节点是以太坊网络中的一种节点，负责执行交易和智能合约的代码。执行层节点运行着以太坊虚拟机（EVM），这是一个全局的、沙盒化的执行环境，可以精确地执行智能合约的字节码。EVM确保了智能合约在不同节点上的执行结果是一致的，从而维护了网络的同步和共识。

执行层节点通过验证和执行交易，来维护以太坊区块链的状态。每个执行层节点都会验证每个区块中的所有交易，并执行这些交易中定义的智能合约代码。这个过程包括计算交易费用、更新账户余额、执行智能合约的函数调用等。执行层节点还负责创建新的区块，并将这些区块广播到网络中的其他节点。

执行层节点与共识层节点（Consensus Layer node）一起工作，共识层节点负责网络的共识机制，例如在以太坊2.0中的权益证明（Proof of Stake, PoS）机制。执行层节点提供了执行交易和合约的功能，而共识层节点则确保了区块的产生和链的安全性。

以太坊客户端软件，如Geth或Parity，通常包含了执行层的功能。这些客户端软件实现了以太坊的规范，并通过点对点网络与其他以太坊客户端通信。不同的客户端软件可能用不同的编程语言编写，但它们都遵循相同的协议规则，因此可以互操作。


[1]  https://ethereum.stackexchange.com/questions/136728/who-runs-the-evm


#### State transition function

以太坊的状态转换函数是以太坊区块链核心概念之一，它描述了如何从当前的全局状态转换到下一个状态。这个过程是以太坊执行交易和智能合约的基础。

在以太坊中，全局状态是由所有账户的当前状态（包括它们的余额、存储的数据和代码）组成的。每当一个交易被执行时，状态转换函数就会被调用来更新全局状态。这个函数接受当前的全局状态和一个交易作为输入，输出一个新的全局状态。

状态转换函数的工作流程大致如下：

1. **验证交易**：首先验证交易的有效性，包括签名的验证和交易发起者账户余额是否足够支付交易费用。
2. **执行交易**：然后根据交易的内容执行相应的操作。这可能包括转移以太币、执行智能合约代码或创建新的智能合约。
3. **更新状态**：执行交易后，更新全局状态以反映交易的结果。这可能包括改变账户的余额、更新存储的数据或更改智能合约的代码。
4. **收取交易费**：最后从交易发起者的账户中扣除交易费用，并将其作为奖励发给矿工或验证者。

以太坊的状态转换函数是确定性的，如果给定相同的输入（当前状态和交易），它总是产生相同的输出（新状态）。这保证了所有节点在处理相同的交易集合时，最终会达成相同的全局状态，从而维护了区块链的一致性和安全性。






### 2024.4.6
#### p2p网络

- P2P网络，又叫点对点网络，是一种去中心化的网络结构，其中每个网络节点既是客户端又是服务器，能够直接与其他节点进行通信和数据交换。这种网络结构允许节点共享资源，如文件、存储空间或带宽，无需依赖中央服务器
- P2P网络的工作原理是基于对等的节点之间直接进行通信和数据交换，而不依赖于中央服务器。在P2P网络中，每个节点既是客户端也是服务器，可以提供和请求服务。这种网络模型允许节点共享文件、处理器运算能力、存储空间等资源。
- 在P2P网络中，节点需要解决如何加入和离开网络、如何在网络中查找和连接到其他节点、以及如何处理节点故障和恢复等问题。为了实现这些功能，P2P网络通常采用特定的通信协议，如Gossip协议，该协议通过节点之间的相互传递信息来实现数据的分发和更新

#### IPFS

1. IPFS（InterPlanetary File System）是一个点对点的分布式文件存储系统，旨在连接所有计算设备具有相同的文件系统。在区块链领域，IPFS常被用来存储不适合直接存储在区块链上的大型数据，例如图片、视频等媒体文件。

2. IPFS的工作原理是将文件分割成小块，每一块都通过加密哈希函数生成一个唯一的哈希值。这些哈希值作为文件内容的唯一标识符，可以用来检索文件。当用户需要获取某个文件时，IPFS网络可以利用这些哈希值找到存储相应文件块的节点，并从多个节点处并行下载，提高了数据的检索效率和系统的健壮性。

3. IPFS使用分布式哈希表（DHT）来跟踪数据块的位置。DHT是一种用于去中心化网络的键值存储系统，它允许网络中的节点高效地找到存储特定数据的其他节点。IPFS中的DHT基于Kademlia算法，这是一种专为P2P网络设计的DHT。

4. 在区块链中，IPFS可以与智能合约结合使用，以实现去中心化的数据存储解决方案。例如，可以将数据存储在IPFS中，而将数据的哈希值和其他相关信息（如交易地址和签名）存储在区块链上。这样，区块链就可以提供一个不可篡改的数据历史记录，而IPFS则负责存储实际的数据内容。

5. IPFS的优势在于它能够解决区块链在数据存储方面的限制，如存储空间和响应时间。通过将数据存储在IPFS上，区块链可以保持轻量级，同时确保数据的不可篡改性和持久性。此外，IPFS的去中心化特性也有助于提高数据存储的安全性和隐私保护。


> 今天了解了p2p网络的工作原理和IPFS的优势，以及它们在区块链领域的应用。


[1] https://icommunity.io/en/what-is-ifps-the-hard-drive-for-blockchain/

[2]  https://www.youtube.com/watch?v=vTIfRgoaCIM


#### Ethereum in 30 minutes video 
本视频是以太坊联合创始人Vitalik Buterin在2022年波哥大Devcon会议上的演讲。在演讲中，Vitalik讨论了以太坊的现状及其未来的发展。

1. 以太坊是一个去中心化的开源区块链平台，可以创建智能合约和去中心化应用程序(dApps)。智能合约是自动执行的合约，其协议条款直接写入代码，允许数字交易自动化和消除中介。

2. Vitalik讨论了以太坊面临的可扩展性挑战以及为解决这些挑战而开发的解决方案，例如分片和第二层扩展解决方案。分片是一种将以太坊网络分割成更小，更易于管理的碎片的方法，称为分片，这允许更大的可扩展性和更快的交易处理。第2层扩展解决方案，如乐观rollup和zk - rollup，建立在以太坊区块链之上，通过链下处理，然后提交给以太坊网络进行最终结算，从而实现更快、更便宜的交易。

3. Vitalik还讨论了以太坊从工作量证明(PoW)共识机制向权益证明(PoS)共识机制的过渡，称为以太坊2.0或Serenity。PoS是一种更节能、更环保的共识机制，它用验证者取代了对矿工的需求，验证者将他们的以太币(ETH)用于验证交易并保护网络。


#### POW AND POS

以太坊的工作量证明（Proof of Work，简称PoW）和权益证明（Proof of Stake，简称PoS）是两种不同的共识机制，用于在去中心化网络中验证交易和创建新区块。

**工作量证明（PoW）**

在PoW系统中，矿工使用GPU和ASIC（专用集成电路）等硬件来解决复杂的数学问题，以此来验证交易并创建新的区块。这个过程被称为挖矿。挖矿的矿工会获得新创建的加密货币和交易费用作为奖励。PoW机制提供了很强的网络安全性，但它非常耗能，因为它需要大量的计算能力。这种能源消耗一直是环境倡导者的批评点，并且对网络的可扩展性也有限制。例如，比特币挖矿大约消耗全球0.5%的能源，但每秒只能处理7笔交易。

**权益证明（PoS）**

为了解决PoW带来的能源消耗和可扩展性问题，以太坊在2022年9月完成了从PoW向PoS的重大转变。PoS机制远不像PoW那样耗能，据估计能够将能源消耗减少高达99%。在PoS系统中，不是通过解决数学问题来竞争创建新区块，而是通过持有并愿意“抵押”一定数量的币的验证者来创建新区块。在以太坊中，验证者抵押的是以太币（ETH）。验证者还参与确认提议区块的有效性。这个过程确保了添加到链上的区块是有效的，并且得到了大多数验证者的同意。验证者通过提议新区块和确认交易的有效性来获得奖励，这些奖励来自交易费用，有时还包括新铸造的加密货币。为了确保验证者诚实行事，PoS系统设有一种惩罚措施，称为“削减”。如果验证者试图批准欺诈交易或行为不端，他们抵押的加密货币的一部分或全部可能会被没收或“削减”。这种机制阻止了不诚实的行为。



### 2024.4.5

#### week0中的Cryptography
- 散列 (Hashing)详解
散列函数像是信息摘要工具，可以将任意长度的输入数据转换成固定长度的输出结果，称为散列值 (Hash 值)。这个过程就像把一堆文件的内容浓缩成一个简短的指纹一样。散列函数有以下关键特性：

1. 易于计算 (Easy to Compute)： 任何人都能快速计算出给定数据的散列值。
2. 抗碰撞 (Collision Resistant)： 对于不同的输入数据，产生相同散列值的可能性极低。就好比不同的人几乎不可能有相同的指纹一样。
3. 单向性 (One-Way)： 仅凭散列值本身，几乎无法推导出原始的输入数据。就如同指纹只能用来验证身份，却无法还原成完整的个人信息。
- 散列函数在密码学领域有着广泛的应用：

- 数字签名 (Digital Signature)： 利用散列函数对数据进行签名，可以确保数据的完整性 and 可靠性 (可靠性 - refers to authenticity in this context)。就好比给重要文件盖上印章，可以验证发送者身份并且确保内容没有被篡改。
- 数据校验 (Data Integrity Check)： 通过比较原始数据的散列值和传输过程中的散列值，可以判断数据是否在传输过程中被修改。就如同核对货物出库时的封条一样，可以确保货物在运输途中没有被掉包。
- 密码存储 (Password Storage)： 网站不会存储用户的原始密码，而是存储密码的散列值。这样即使黑客窃取了数据库，也无法获得用户的明文密码。就好比指纹识别系统只存储指纹的特徵，而不是整只手的外貌。
常用的散列函数包括 MD5、SHA-1 和 SHA-2 家族。

- 公钥密码学 (Public Key Cryptography)详解
公钥密码学采用密钥对 (Key Pair) 的方式进行加密和解密。密钥对包含公钥 (Public Key) 和私钥 (Private Key)。形象地比喻，公钥就像是邮箱的地址，所有人都可以知道；而私钥就像邮箱的钥匙，只有持有者才能打开邮箱。

- 公钥密码学的工作流程如下：

加密 (Encryption)： 信息发送者使用接收方的公钥对信息进行加密。 任何人都可以用公钥加密信息，但是只有持有私钥的人才能解密。
解密 (Decryption)： 信息接收者使用自己的私钥解密密文，还原成原始信息。 因为只有持有私钥的人才能解密，所以可以确保信息的保密性。

常用的公钥密码算法包括 RSA 算法

#### Merkle 树详解
- Merkle 树是一种树形数据结构，常用于高效验证大型数据的完整性。它利用密码学中的散列函数来构建，具有以下特点：

1. 数据完整性校验 (Data Integrity Verification)： 可以方便地校验数据块 (Data Block) 的完整性，而无需检查整个数据。
2. 去重存储 (Deduplication)： 可以识别并消除数据中的重复部分，节省存储空间。
##### Merkle 树的结构
Merkle 树由节点 (Node) 组成，每个节点包含以下信息：

- 数据块哈希 (Hash of Data Block)： 对数据块计算所得的散列值。
- 子节点哈希 (Hashes of Children Nodes)（可选）： 对于非叶子节点，包含其子节点的哈希值。
Merkle 树的层级结构清晰，叶子节点代表原始数据块的哈希值，非叶子节点的哈希值由其子节点的哈希值计算得到。计算方法通常采用一种称为 "Merkle-Damgård Hash Function" 的技术，例如 SHA-256。

##### Merkle 树的工作原理

1. 在Merkle树中,每个叶子节点都标记了一个数据块的加密哈希值,而每个非叶子节点则标记了其子节点标签的加密哈希值。

2. 叶子节点的数据先被哈希,然后成对分组。每对节点的哈希值再被哈希,生成它们的父节点。重复这个过程,直到只剩下一个根节点,称为Merkle根。

3. Merkle树通常使用二叉树结构,即每个节点最多有2个子节点。但也可以使用多叉树,每个节点有多个子节点。

4. 为了验证某个叶子节点是否属于Merkle树,只需计算沿着该叶子到根的路径上的哈希值,数量与树的深度成正比。这比在哈希列表中逐个检查每个节点要高效得多。

5. 如果Merkle树的任何部分发生变化,都会传播到根节点。通过比较根哈希,可以快速检测出数据是否被篡改。

6. Merkle树的搜索、插入、删除操作复杂度为O(logn),空间复杂度为O(n),其中n是叶子节点数。

Merkle树利用哈希指针构建一个树形结构,可以高效地验证大型数据结构的完整性,广泛用于区块链、分布式系统、版本控制等领域中。它通过递归哈希的方式,在保证安全性的同时提供了更好的性能。

[1] https://www.youtube.com/watch?v=3AcQyTs_Es4&t=0

[2] https://brilliant.org/wiki/merkle-tree/

[3] https://www.geeksforgeeks.org/blockchain-merkle-trees/

[4] https://en.wikipedia.org/wiki/Merkle_tree

### 2024.4.3
