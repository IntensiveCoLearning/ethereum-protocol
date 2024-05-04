# BruceXu

Hi guys, I'm Bruce, I'm learning Ethereum Protocol. I'm good at Web development. Follow me on Twitter: <https://twitter.com/brucexu_eth>, my telegram: <https://t.me/brucexu_eth>.

## Notes

### 5.4

今天主要看了下 Pyspec 尝试跑了一下。

### 5.3

[Week 6](https://epf.wiki/#/eps/week6-dev)

共识层的 specs，信息量挺大的 https://github.com/ethereum/consensus-specs?tab=readme-ov-file 感觉包括了各种变更历史。

TODO 或许可以通过标准的变更来写一下以太坊的进化历史？

How to use Executable Consensus Pyspec By Hsiao-Wei Wang

协议可以通过 Python Pyspec 运行？大概流程：

1. 用 Pyspec markdown 文件实现新的功能
2. 附带 test vector suite @ethereum/consensus-spec-tests
3. CL 实施 + 测试

TODO 咋运行？跑一下看看

### 5.2

```
# Install htop or btop for monitoring
# https://github.com/aristocratos/btop

# Install gpg for verifying binary
sudo apt install gpg

# Download geth here: https://geth.ethereum.org/downloads
# Download lighthouse here: https://github.com/sigp/lighthouse/releases

# Optional: download OpenPGP Signatures and import
gpg --import armored-keys.asc

# Optional: verify tar.gz.asc
gpg --verify geth-linux-amd64-1.14.0-87246f3c.tar.gz.asc

# Unzip tar.gz
tar -xzf xxx.tar.gz

# Run geth, you could check the logs
./geth

# Create dirs for data so that you can check original data easier
mkdir geth-data

# Tips: by using ./geth --help you will see a lot of parameters, feel free to take look on them

# Generate jwtsecret
openssl rand -hex 32 > /tmp/jwt

# Set some parameters to start geth for holesky testnet and enable RPCs
./geth --holesky --datadir geth-data --syncmode snap --http --http.port 8545 --authrpc.jwtsecret /tmp/jwt --authrpc.port 8551

# Check the logs, EL should be started

# Start CL
# Same, you can check the parameters by using --help
./lighthouse --help

# Start beacon node with checkpoint
./lighthouse bn --network holesky --execution-endpoint http://localhost:8551 --execution-jwt /tmp/jwt --http --checkpoint-sync-url https://holesky.beaconstate.ethstaker.cc/


# Setup ephemery for a quicker test
# Download ephemery from https://github.com/ephemery-testnet/ephemery-genesis/releases
wget https://github.com/ephemery-testnet/ephemery-genesis/releases/download/ephemery-CHECKLATEST/testnet-all.tar.gz
mkdir ephemery && tar -xzf testnet-all.tar.gz -C ephemery
./geth --datadir geth-ephemery init ephemery/genesis.json
source ephemery/nodevars_env.txt
./geth --datadir geth-ephemery --authrpc.jwtsecret=/tmp/jwt --bootnodes $BOOTNODE_ENODE --http
./lighthouse bn -t ephemery --execution-endpoint http://localhost:8551 --execution-jwt=/tmp/jwt --boot-nodes=$BOOTNODE_ENR_LIST

# Go get some curl from https://ethereum.org/en/developers/docs/apis/json-rpc/ and test the RPC API
# Tip: add --http to geth to enable rpc
curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":67}' localhost:8545 | jq
curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":67}' localhost:8545 | jq

```

- lighthouse 可以运行 bn or vc，bn 是 beacon node，vc 是 validator node。
- 可以使用 ephemery https://github.com/ephemery-testnet 提升学习下载的速度 https://ephemery.dev/
- genesis.json 记录了一些账户 balance 等初始信息
- 可以通过 MetaMask 添加本地 RPC 来连接到测试网
- Check nodes https://www.ethernets.io/

测试网的注册 validator 流程：

- 领一下测试币，至少 32 个 https://github.com/ephemery-testnet/ephemery-resources?tab=readme-ov-file#faucets
- 打开 launchpad https://github.com/ephemery-testnet/ephemery-resources?tab=readme-ov-file#validators 然后 become a validator

TODO，从 1:44m 开始的，但是那个 deposit cli 失效了。

- Beacon Deposit Contract https://etherscan.io/address/0x00000000219ab540356cbb839cbe05303d7705fa

TODO 创建一个中文地区的 eth-clients.github.io

### 5.1

```
# Install htop or btop for monitoring
# https://github.com/aristocratos/btop

# install gpg for verifying binary
sudo apt install gpg

# optional: download OpenPGP Signatures and import
gpg --import armored-keys.asc

# verify tar.gz.asc
gpg --verify geth-linux-amd64-1.14.0-87246f3c.tar.gz.asc

# unzip tar.gz
tar -xzf xxx.tar.gz


```

### 4.30

[Node workshop](https://epf.wiki/#/eps/nodes_workshop)

还没看完没搭建起来，打算按照视频写一个简单的命令行版本提个 pr 到 wiki 上面，比较方便操作一些。

### 4.29

[Node workshop](https://epf.wiki/#/eps/nodes_workshop)

2 个小时视频，需要跟着搭建一个 node 跑一遍。

### 4.28

Vitalik 版本的路线图定义，但是被 EF 迪士尼了。

- The Merge: upgrades relating to the switch from proof-of-work to proof-of-stake
- The Surge: upgrades related to scalability by rollups and data sharding
- The Scourge: upgrades related to censorship resistance, decentralization and protocol risks from MEV
- The Verge: upgrades related to verifying blocks more easily
- The Purge: upgrades related to reducing the computational costs of running nodes and simplifying the protocol
- The Splurge: other upgrades that don't fit well into the previous categories.

- Single slot finality - Instead of waiting for fifteen minutes, blocks could get proposed and finalized in the same slot. This is more convenient for apps and much more difficult to attack.
- Proposer-builder separation - Splitting the block building and block proposal tasks across separate validators creates a fairer, more censorship resistant and efficient way for Ethereum to come to consensus.
- Secret leader election - Clever cryptography can be used to ensure that the identity of the current block proposer is not made public, protecting them from certain types of attack.
- Statelessness - Stateless clients will be able to verify new blocks without having to store large amounts of data. This will provide all the benefits of running a node with only a tiny fraction of today’s costs.

- SLE solution
  - EIP 7441 Upgrade block proposer election to Whisk
    - Upgrade the block proposer election mechanism to Whisk, a single secret leader election protocol
    - Allow elected block proposers to remain private until block publishing, to prevent DoS attacks
  - Currently, SLE is relatively in low priority. But priority can change if such DoS attacks happen.

TODO 目前 ethereum 没有 DDoS 攻击吗？目前是如何应对的？

- Single Slot Finality (SSF)

  - Current problem
    - The current finality time is after 2 epochs (c.12.6min) as too many signatures to check and aggregate.
    - And the devs want to enhance the finality speed into 1 slot (12s)
  - Solution path
    - Fewer validators through Max EB (EIP 7251)
    - Fewer active validators eg. rotating cap
    - Way fewer validators (8,192) + Distributed validators tech (DVT)
    - Better signature aggregation schemes
  - Vitalik's blog on Paths towards SSF: https://notes.ethereum.org/@vbuterin/single_slot_finality

- Proposer/ Builder Separation (PBS)

  - Current problem: MEV is inevitable, and untamed MEV markets will hurt solo stakers
  - Goal: Minimize the choices validators have to make and reduce incentive to specialized validators
  - Solution
    - Current solution: Out-of-protocol with MEV boost, where relays act as trusted brokers
    - Future solution: Enshrined PBS (ePBS), which removes relays and allows MEV burning to smooth the staking yield
    - Future solution: Inclusion list, which puts constraints on builders and reduces censorship by forcing transactions inclusion

- Fully SNARKed Ethereum
  - Snarkify light client protocol (sync committee transitions)
  - Snarkify all beacon chain transitions (signatures, balance changes, etc.)
  - Snarkify verkle state across proofs/ block witness
  - Eventually snarkify all EVM execution: zkRollups are working on zkEVMs, that could be brought back to the core protocol in the future.

TODO 整个以太坊 ZK 化是一个大体方向。

### 4.27

[https://ethereum.org/en/roadmap/](https://ethereum.org/en/roadmap/)

路线升级主要是四个目的和方向：

- 更好体验
  - 如何管理 keys 和钱包，以及发送交易，gas 等，使用 Ethereum 需要得到简化
    - 例如使用 AA 钱包
      - 保护钱包的 key 不会丢失或者被盗，然后有更好的 fraud 检测
  - 每个人都可以运行 node，更快的、私密的、无需可的使用以太坊，目前大家还是需要使用别人的 node 提供服务
  - 目前需要非常大的硬件设施才可以运行
  - 如何让大家可以更简单的更低成本的运行 node 是个很重要的课题
    - 使用 Verkle Tree 可以有跟高效的数据存储方式，节约空间
    - statelessness 和 data expiry 可以删除一些不用的老数据
      - Full statelessness is still in the research phase and is likely several years away from being implemented.
    - light nodes 提供了一些功能，可以运行在 mobile phones 或者简单的 browser apps
- 面向未来
  - 确保未来的一些问题和技术不会影响到以太坊的运行
  - 抵抗量子计算，量子计算可能会对一些加密算法实现破解
    - PoS 使用的签名 schema BLS 可以被量子计算机攻破，替代方案不够高效
    - KZG commitment schemes 也是 quantum-vulnerable 的
      - 目前使用 trusted setups 来解决，很多用户生成不可被逆向的随机数
      - 最好的办法就是直接创建安全的量子加密算法
        - STARK-based 和 lattice-based signing 是两个替代 BLS scheme 的潜在方法

ETH R&D Discord link：https://t.co/EqHJvFlv9E

### 4.26

[https://ethereum.org/en/roadmap/](https://ethereum.org/en/roadmap/)

路线升级主要是四个目的和方向：

- 更加便宜
  - 通过 Rollups 和 L2 来实现，简单原理就是把很多交易执行，然后把结果打包压缩提交到 Ethereum，保持数据公开可用，大家可以方便验证
  - Proto-Danksharding 就是提供一个临时的 blob 存储，这样减少存储的成本
  - Danksharding 分片存储，加密验证部分数据的完整性和可靠性，依赖 PBS 和 DAS，还需要几年的时间开发。TODO 可以研究如何推进 Danksharding 的升级速度
  - 去中心化 rollups，sequencers 是中心化的，存在 censorship 的问题，涉及到 validity/fraud proofs 等
- 更加安全
  - 转成 PoS 系统，而且开放了 withdraw 什么的，so far so good
  - 减少攻击的可能性，比如更好的 fork-choice 算法 view-merge 然后就是降低出块时间
  - Single slot finality (SSF) is a way to minimize the finalization delay
  - 抗审查，通过 DVT distributed validator technology 技术。比如切割 validator keys 在不同的系统，而不是一个 operator 维护多个 validators
  - PBS 也有助于抗审查，避免 validator 被操控或者贿赂
  - 抵抗针对 validator 的 DoS，比如 secret leader election SLE，防止 block proposer 被提前知道（因为要提前进行选举）
    - This works by continually shuffling a set of cryptographic commitments representing candidate block proposers and using their order to determine which validator is selected in such a way that only the validators themselves know their ordering in advance.

### 4.25

[Study Group Week 5 | Research and Roadmap](https://epf.wiki/#/eps/week5)

Raise max effective balance (MaxEB)
• Today: Minimum 32 ETH, maximum 32 ETH
After MaxEB: Minimum 32 ETH, maximum 2048

- automatic compounding
- fewer validators for the same amount of stake (lower overhead)

Verkle-prove that Bob owns 5 ETH
Every node is now a polynomial commitment over its children

Verkle trees
• Much shorter state proofs
• Wider tree (256 vs 16 siblings)
• ZK-friendly proofs
• Allow stateless validators
(no history needed – instant sync)
• Light clients become even lighter
• Lower dev reliance on centralized indexers

TODO 使用 JS 实现一个 merkle tree 和 verkle trees 试试看

SNARKify all the things
• Light client protocol (sync committee transitions)
• All beacon chain transitions (signatures, balance changes, etc.)
• Verkle state access proofs / block witnesses
• Eventually all EVM execution (thank you zkRollups!)
zkEVM opcode/precompile
• Verify EVM execution proof inside the EVM (or inside an EVM execution proof…)

### 4.24

[Study Group Week 5 | Research and Roadmap](https://epf.wiki/#/eps/week5)

TODO https://hackmd.io/@vbuterin/stark_aggregation

感觉路线图这个有点硬核，可以考虑出一个精品教程之类的，专门进行研究。

All rollup data must be available on Layer 1 (for those who need/want it)
• Force L2 transaction inclusion (i.e. to exit back to L1)

DAS 需要考虑验证信息的问题，确保被分片切割的信息是准确没有丢失的。

Quantum-proof blobspace
• KZG drawbacks: Not quantum-proof and required a trusted setup (>140k contributors)
• Eventually hot-swap KZG for something based on STARKs or Lattices

Cross-rollup interopability
• Establish standards between rollups
• Based rollups, preconfirmations, shared sequencing

感觉挺多要做的，比如 blob 的存储格式定义，方便其他 L2 互操作什么的。不过也有点难搞，涉及到跨链了。或许可以通过 blob 进行跨链？这样的场景有点类似使用 localstorage 实现跨域了。

TODO Endgame：Centralized production, decentralized validation, strong anti-censorship protection https://vitalik.eth.limo/general/2021/12/06/endgame.html

### 4.23

[Week 4 EPFsg Test & Security Notes](https://ab9jvcjkej.feishu.cn/docx/R1IOdEkDxoiNNHxvA6sclfITnLB)

mermaid 这个代码画图工具可以学习了解一下，看看能不能构建出以太坊的路线图。

- As the EVM testing needs Geth implementation to fill the test, what if there is a bug in Geth's code or how to ensure there is no bug?
  - Ideally, devs don't want to depend on Geth's implementation. Devs are currently working on another specs-oriented repo, so that in the future the test filing will not be dependent on Geth.

TODO 这里是个可以做的机会和事情。

[Study Group Week 5 | Research and Roadmap](https://epf.wiki/#/eps/week5)

● Merge: Better Proof of Stake
● Surge: More data (availability) for rollups
● Scourge: Less MEV downsides
● Verge: Easier verification
● Purge: Simpler protocol
● Splurge: Miscellaneous goodies

Single Slot Finality

• From 12.6 minutes to 12 seconds
• Main problem: Too many signatures to check and aggregate

Solution paths:
• Fewer validators (MaxEB)
• Fewer active validators
• Way fewer validators (8192) + Distributed Validators Tech
• Better signature aggregation schemes

### 4.22

[Ethereum Testing and Security](https://github.com/eth-protocol-fellows/protocol-studies/blob/main/docs/eps/presentations/week4.pdf)

测试就是测试客户端的行为是不是跟标准一致。在同样的输入下，有相同的输出。

EVM 测试有几个大的测试内容：

- Pre-State
- Environment
- Transactions
- Post-State

基本上就是不同阶段的各种类型的内容，加上 Fuzzy 随机生成进行测试，看结果。主要就是用 JSON 等格式来输入。

主要测试工具代码：https://github.com/ethereum/tests

感觉集成测试会是一个比较难的问题，这种去中心化网络的测试怎么做？如何构建和模拟一个去中心化网络和相关情况？

测试网大概分三类：

- Devnets：PoC 验证 hard-forks
- Shadow-Forks：跟 mainnet 差不多，包括一些 hard-fork 参数，类似预发环境
- Public Testnets：公开测试网，基于新的共识或者运行机制让大家测试

### 4.21

开始学习 Week4，混了个 PR https://github.com/eth-protocol-fellows/protocol-studies/pull/197

### 4.20

[PROOF-OF-STAKE (POS)](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/)

一个交易的工作流程：

- 用户创建交易，通过 JSON-RPC API 发送到某个节点
- 节点收到之后，进行初步验证
- 合法的 txs 会被加入到 local mempool，然后通过 gossip network 广播到其他节点进行同步
- 通过 RANDAO 随机算法选出来的 block proposer 节点来构建和广播 block
  - 从本地 mempool 拿出来然后生成 block
- 其他节点收到构建的 block，然后本地验证是否正确，然后 attests 存储到本地数据库
- 大概通过两个检查点，可以确定当前交易 finalized 了

Two primary behaviors can be considered dishonest: proposing multiple blocks in a single slot (equivocating) and submitting contradictory attestations.

[The Beacon Chain Ethereum 2.0 explainer you need to read first](https://ethos.dev/beacon-chain)

A block proposer is a validator that has been pseudorandomly selected to build a block.

At every epoch, a validator is pseudorandomly assigned to a slot.

An attestation is a validator’s vote, weighted by the validator’s balance. Attestations are broadcasted by validators in addition to blocks.

Validators also police each other and are rewarded for reporting other validators that make conflicting votes, or propose multiple blocks.

The contents of the Beacon Chain is primarily a registry of validator addresses, the state of each validator, and attestations. Validators are activated by the Beacon Chain and can transition to states, briefly described later in Beacon Chain Validator Activation and Lifecycle.

A checkpoint is a block in the first slot of an epoch. If there is no such block, then the checkpoint is the preceding most recent block. There is always one checkpoint block per epoch. A block can be the checkpoint for multiple epochs.

Epoch boundary blocks (EBB) are a term in some literature (such as the Gasper paper, the source of the diagram above and a later one), and they can be considered synonymous with checkpoints.

A vote that is made by ⅔ of the total balance of all active validators, is deemed a supermajority. Pedagogically, suppose there are three active validators: two have a balance of 8 ETH, and a sole validator with a balance of 32 ETH. The supermajority vote must contain the vote of the sole validator: although the other two validators may vote differently to the sole validator, they do not have enough balance to form the supermajority.

挺中心化的。

When finalizing a checkpoint, there is no limit to the number of blocks that can be finalized. Although finality is only computed at epoch boundaries, attestations are accumulated at each block, as described in alternate narratives “What could have happened from genesis to the head” below.

Validators are rewarded the most when their attestation is included on-chain at their assigned slot; later inclusion is a decaying reward.

To give validators time to prepare, they are assigned to committees one epoch in advance. Proposers are only assigned to slots once the epoch starts. Nonetheless, secret leader election research aims to mitigate attacks or bribing of proposers.

As a staker concerned about how much ETH you may lose, it’s close to a mirror of how much you can earn. For example, if a validator stands to make 10% in a year on attester rewards, a (honest) validator stands to lose 7.5% if they do the worst job possible.

A double proposal is a proposer proposing more than one block for their assigned slot.

Similarly, an LMD GHOST double vote is a validator attesting to two different Beacon Chain heads for their assigned slot.

Each validator needs a balance of 32 ETH to get activated. A user staking 32 ETH into a deposit contract on Ethereum mainnet, will activate one validator.

The Beacon Chain deactivates (“forced exit”) all validators whose balance reaches 16 ETH; stakers will be able to withdraw any remaining validator balance likely in 2023.

Validators can also “voluntary exit” after serving for 2,048 epochs, around 9 days.

In any voluntary or forced exit, there is a delay of four epochs before stakers can withdraw their stake. Within the four epochs, a validator can still be caught and slashed. An honest validator’s balance is withdrawable in around 27 hours. But a slashed validator incurs a delay of 8,192 epochs (approximately 36 days).

To avoid large changes in the validator set in a short amount of time, there are mechanisms limiting how many validators can be activated or exited within an epoch. For example, these make it more difficult to activate many validators quickly to attack the system.

### 4.19

[PROOF-OF-STAKE (POS)](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/)

In regard to the Ethereum blockchain, the process is formalized, and reaching consensus means that at least 66% of the nodes on the network agree on the global state of the network.

In the rare situations where multiple blocks are in the same position near the head of the chain, there is a fork-choice mechanism that selects blocks that make up the 'heaviest' chain, measured by the number of validators that voted for the blocks weighted by their staked ether balance.

PoS: Validators create blocks. One validator is randomly selected in each slot to be the block proposer. Their consensus client requests a bundle of transactions as an 'execution payload' from their paired execution client. They wrap this in consensus data to form a block, which they send to other nodes on the Ethereum network. This block production is rewarded in ETH. In rare cases when multiple possible blocks exist for a single slot, or nodes hear about blocks at different times, the fork choice algorithm picks the block that forms the chain with the greatest weight of attestations (where weight is the number of validators attesting scaled by their ETH balance).

The weight is the accumulated sum of validator votes, weighted by validator staked-ether balances.

Ethereum uses a consensus mechanism known as Gasper that combines Casper FFG proof-of-stake with the GHOST fork-choice rule.

Proof-of-stake is a way to prove that validators have put something of value into the network that can be destroyed if they act dishonestly. In Ethereum's proof-of-stake, validators explicitly stake capital in the form of ETH into a smart contract on Ethereum. The validator is then responsible for checking that new blocks propagated over the network are valid and occasionally creating and propagating new blocks themselves. If they try to defraud the network (for example by proposing multiple blocks when they ought to send one or sending conflicting attestations), some or all of their staked ETH can be destroyed.

### 4.18

[Week3 slide](https://github.com/eth-protocol-fellows/protocol-studies/blob/main/docs/eps/presentations/week3_presentation.pdf)

● Blockchains are useful because they can generate digital scarcity
● Use this property to emulate all sorts of physical assets in digital realm
○ Money, tokens, property rights
○ We are still figuring out what all is possible
“Scarcity” means that there are only ever N coins at one time, a user can’t
spend more coins than they have

### 4.17

[Week 3 EPFsg Consensus Layer Notes](https://ab9jvcjkej.feishu.cn/docx/X7Ard9UlPoj2lmxKMvucfNubnTb)

Ethereum consensus mechanism

- Validator makes cryptographic signature over the state of the chain
- Different types of attestations
  - LMD GHOST vote: Validator attests to the beacon chain head
  - Casper FFG vote: Validator attests to the checkpoint in its current epoch

关键词：

- Slot
  - Every 12 seconds there will be a new slot, and every slot will have a block
  - 分成三个步骤，一个 4s。首先 proposer 广播 block，然后 attestation，然后一组 validators 广播结果
- Epoch
  - Each epoch has 32 slots. The reason behind creating the epoch is to reduce the frequency of consensus processing, so that it doesn't need to happen in every slot
  - Heavier processing is usually done at the epoch boundary, incl. slashing, rewards info etc.
- Committee
  - Validators 随机选举
  - Each validator will make one attestation per epoch. The exact slot the validator is assigned is determined by the protocol through RANDAO.
- Finality
  - Finality means that a tx is part of a block that can't change.
  - Justification: When an epoch ends, if its checkpoint has gathered a 2/3 supermajority, the checkpoint gets justified.
  - Finality: When a checkpoint is justified, the previous checkpoint that is already justified becomes finalized.
- The validator selection is fixed two epochs in advance as a way to protect against certain kinds of seed manipulation.
- Gasper in the context of finality and finding the canonical chain?
  - Gasper is the combination of Casper-FFG and LMD-GHOST fork choice algorithm (Gasper)
    - Casper is the mechanism that upgrades certain to finalized, so that new entrants can be confident that they are syncing the canonical chain.
    - LMD-GHOST is the fork choice algorithm that uses accumulated votes to ensure that nodes can easily select the correct one when forks arise in the blockchain.
- Some of the important things on the roadmap of Ethereum

  - SSF (single slot finality): Aim to get finality in a single slot
    - Vitalik post on SSF: https://notes.ethereum.org/@vbuterin/single_slot_finality
    - Roadmap blog: https://ethereum.org/en/roadmap/single-slot-finality/
  - SSLE (single secret leader election): Aim to have proposer selection in secret
    - Research link: https://ethresear.ch/t/simplified-ssle/12315
    - Roadmap blog: https://ethereum.org/en/roadmap/secret-leader-election/
  - Max EB (max effective balance): Aim to increase the effective balance of Ethereum validators at 32 ETH
    - Research link: https://ethresear.ch/t/increase-the-max-effective-balance-a-modest-proposal/15801

- TODO RANDAO 算法是怎么运行的？
-

### 4.16

[Week 3 EPFsg Consensus Layer Notes](https://ab9jvcjkej.feishu.cn/docx/X7Ard9UlPoj2lmxKMvucfNubnTb)

Byzantine fault tolerance BFT 拜占庭容错在区块链的应用非常重要，因为它要实现两个特性：

- 一致性：所有诚实的节点需要达成一致
- 可用性：损失部分节点不影响决策

在区块链中，去中心化的网络有很多节点，服务器都有故障的可能性，所以需要拜占庭容错。

Two-phase commit (2PC)

其实是分布式数据库用来确保事务一致性的协议，协调所有节点同时提交或者回滚事务，达成一致的状态。

- 第一阶段：一个节点问其他节点，是否可以接受新的 txs。大家需要响应 yes or no，超过 2/3 的 yes 即可确认完成该阶段。相当于号召一下，大家是否响应你的号召。
- 第二阶段：这个节点命令其他节点 commit txs 接受你的指示，如果有人拒绝了，可能就变成 abort 了。

这种方式在高性能和高可用的环境下可能不是最佳选择，在网络传输的过程中，容易形成阻塞或者失败恢复等问题。

PBFT consensus algorithm allows a distributed system to reach a consensus even when a small amount of nodes demonstrate malicious behavior.

Bitcoin is considered the 1st solution to solve the Byzantine General problem

- The system can scale to unlimited node count
- Open & permissionless participation
- Use PoW mechanism to reach consensus

通过 PoW 算法选举出 coordinator 来命令大家，其他节点通过密码学来验证确实是要听它的，然后进行响应和保存新的数据。根据网络情况，会动态调整 PoW 的难度等。在通过激励 BTC 的方式激励大家，确实是自成体系的一套运转规则，有点鬼斧天工的意思。

TODO 这里面涉及到哪些学科或者知识才能产出来这样的？

- 技术：分布式技术、P2P、密码学
- 经济学：了解货币的发展、对经济危机不满

TODO 还有什么跨学科的知识？不得不说，人才还是需要双 T 或者多 T 的，不然根本没法交织在一起进行创新。

### 4.15

[Week 3 EPFsg Consensus Layer Notes](https://ab9jvcjkej.feishu.cn/docx/X7Ard9UlPoj2lmxKMvucfNubnTb)

scarcity 的意思是独一无二的东西。digital scarcity 独一无二的数字品。

Blockchain enables a way to create digital scarcity

What's Byzantine fault tolerance (BFT)?

- Byzantine fault tolerance (BFT) is the property of a system that is able to resist the class of failures derived from the Byzantine Generals' Problem. This means that a BFT system is able to continue operating even if some of the nodes fail or act maliciously.

### 4.14

#### [lecture7](https://cs251.stanford.edu/lectures/lecture7.pdf)

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/054a8bfb-d776-4ab1-aea4-5623761332e9)

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/8926f81d-e2fe-4474-9a09-88a871161614)

Finished Week2

### 4.13

#### [Study Group Week 2 | Execution Layer](https://epf.wiki/#/eps/week2)

[NODES AND CLIENTS](https://ethereum.org/en/developers/docs/nodes-and-clients/)

Archive nodes are full nodes that verify every block from genesis and never delete any of the downloaded data.

Instead of downloading every block, light nodes only download block headers. These headers contain summary information about the contents of the blocks.

The light nodes do not participate in consensus (i.e. they cannot be miners/validators), but they can access the Ethereum blockchain with the same functionality and security guarantees as a full node.

[Portal Network](https://www.ethportal.net/overview)

Decentralized API access to an Ethereum archive node with near-zero sync time and minimal hardware requirements!

The Portal Network is a peer-to-peer protocol that runs parallel to Ethereum. Ethereum data is distributed across the Portal Network, instead of being copied in every individual node. This allows users to access Ethereum data with minimal hardware and bandwidth requirements and almost instant syncing.

The Portal Network is actually several peer-to-peer networks (state, beacon, and history networks) that can be accessed using a Portal Network client. Each of these networks stores a specific subset of the data stored by an Ethereum full node. Each individual Portal client only stores a tiny fraction of each type of data. However, the Portal Network (the complete set of nodes) stores all the historical Ethereum data spanning from genesis right up to within a block or two of the head of the chain.

#### [lecture7](https://cs251.stanford.edu/lectures/lecture7.pdf)

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/11e5780e-bc37-44b0-b63a-ea11c1b01df0)

### 4.12

#### [Study Group Week 2 | Execution Layer](https://epf.wiki/#/eps/week2)

[NODES AND CLIENTS](https://ethereum.org/en/developers/docs/nodes-and-clients/)

A node has to run two clients: a consensus client and an execution client.

There is also a third piece of software, known as a 'validator' that can be added to the consensus client, allowing a node to participate in securing the network.

There are different classes of full node - some start from the genesis block and verify every single block in the entire history of the blockchain. Others start their verification at a more recent block that they trust to be valid (e.g. Geth's 'snap sync'). Regardless of where the verification starts, full nodes only keep a local copy of relatively recent data (typically the most recent 128 blocks), allowing older data to be deleted to save disk space. Older data can be regenerated when it is needed.

### 4.11

#### [week2 notes](https://ab9jvcjkej.feishu.cn/docx/BRDdd8kP9o00a2x6F4scRo0fnJh)

STF

- [insertBlockWithoutSetHead](https://github.com/ethereum/go-ethereum/blob/master/core/blockchain.go#L1645) 插入到 blockchain，上面有详细的逻辑。简单的说就是检查、执行、存储

代码解读部分跳过，需要构建 debug 环境，借助单元测试 debug 一边比较容易了解具体逻辑。

- Example on stack machine simulation: https://www.evm.codes/playground
- The EL operates on devp2p, which is the bespoken protocol of Ethereum.

- Historical data: 3 methods to get the historical data from Ethereum
  - GetBlockHeaders: Require peer to return a blockheaders message
  - GetBlockBodies: Request block body data by hash
  - GetReceipts: Require peer to return a receipts message containing the receipts of the given block hashs

#### [Study Group Week 2 | Execution Layer](https://epf.wiki/#/eps/week2)

NODES AND CLIENTS

[ETHEREUM VIRTUAL MACHINE (EVM)](https://ethereum.org/en/developers/docs/evm/)

- At any given block in the chain, Ethereum has one and only one 'canonical' state, and the EVM is what defines the rules for computing a new valid state from block to block.
- It therefore is quite helpful to more formally describe Ethereum as having a state transition function
- In the context of Ethereum, the state is an enormous data structure called a modified Merkle Patricia Trie, which keeps all accounts linked by hashes and reducible to a single root hash stored on the blockchain.
- The EVM executes as a stack machine with a depth of 1024 items. Each item is a 256-bit word, which was chosen for the ease of use with 256-bit cryptography (such as Keccak-256 hashes or secp256k1 signatures).
  - During execution, the EVM maintains a transient memory (as a word-addressed byte array), which does not persist between transactions.
  - Contracts, however, do contain a Merkle Patricia storage trie (as a word-addressable word array), associated with the account in question and part of the global state.
  - All implementations of the EVM must adhere to the specification described in the Ethereum Yellowpaper. 可以跑一下 JS 的 EVM 了解下执行原理 https://github.com/ethereumjs/ethereumjs-monorepo/tree/master/packages/evm 或者自己构建一个最简单的 EVM 写个教程

[MERKLE PATRICIA TRIE](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/)

TODO

### 4.10

#### 核心路线图简介

- The Merge：已经完成，从 PoW 到 PoS 的共识转换，同时还拆分了 EL 和 CL，开启模块化。目前在关注 Single Slot Finality。
- The Surge：聚焦 scaling，扩展。通过 Dencun 升级，EIP-4844 开始第一阶段，第二阶段可以继续提升。
- The Scourge：关注经济学，包括面向 MEV、Censorship Resistance 以及其他 economics 课题。
- The Verge：关注加密学和迁移 Merkle tree 到 Verkle tree 数据结构。
- The Purge：通过去掉一些历史状态来实现轻量级的运行方式。
- The Splurge：fix 剩余的东西。

最终目的就是解决去中心化三角的困境。坑越挖越多，没有什么尽头。

#### [week2 notes](https://ab9jvcjkej.feishu.cn/docx/BRDdd8kP9o00a2x6F4scRo0fnJh)

Block validation

- CL
  - process_execution_payload：验证 block 是否 valid，发给 EL 去 EVM 里面执行验证
  - 调用 EL 的 STF 方法，验证不通过 block 被 reject
- EL
  - 通过 STF（State Transition Function）方法来验证和执行数据
  - 验证 header 是不是有问题
  - 丢到 vm 里面去执行，包括 tx 的信息和 state（TODO 这个 state.StateDB 是 World State 还是 Accounts State，last known valid state 是什么意思？）
  - 没报错就算是成功啦

Block building

- build function
  - 参数：环境变量和通用信息、mempool，按照 value 排序的 txns、StateDB
  - 不断增加 tx 到 block 的 gasUse limit，大概是 30M
  - 持续的 Pop() 拿到 next best tx 然后加入到 block 中直到 no gas 或者 tx pool 空了（TODO 那挂很多低 gas 的 tx 没法执行，岂不是会撑爆 mempool？）
    - 只有 tx invalid 的时候才会被 reject
  - Finalize function 合并成一个 block
- 问题和挑战
  - 如何加密 mempools 目前暂时没有想法和进展。目的是为了减少 mempool 交易的信息泄漏，因为拿到信息之后，可以通过更高的 gas 来抢跑等，或者出现一些隐私的问题

STF

- newPayload 源代码 <https://github.com/ethereum/go-ethereum/blob/master/eth/catalyst/api.go#L524>

TODO

### 4.9

#### [Inevitable Ethereum](https://inevitableeth.com/home/ethereum/world-computer)

先提了一下人类演进，提出了人类协作以及通过钱作为协作的介质，这个角度挺有意思的。钱是人类分配时间、资源、能量的重要协调工具。所以延伸到区块链的价值，可以提供比较高效可靠的资金相关。

EVM 就是一个图灵完备的状态机。

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/95468177/24cba54d-53c7-4330-b970-6290e048e5e7)

Printing Press Revolution

印刷机的出现，引发了社会变革。实际上是加速了人类的知识和思想传播，引发了更多思考带来的。区块链最大的问题就是人们理解区块链需要时间。

Statelessness is a nuanced topic worth reading more about, but the idea can be summarized as "using cryptography to trustlessly access the EVM without having to store it locally."

The Portal Network is (will be) an independent network of computers dedicated to capturing and serving the data needed to serve light clients.

Light clients will query the Portal Network for proofs, mempools anything a its needs to verify the hashes created by Ethereum.

TODO ultra sound money 的背景了解一下

In the Ethereum endgame, ETH is the global currency of trustless trust. ETH is how we can coordinate without centralization. Ethereum is inevitable.

#### [week1](https://epf.wiki/#/eps/week1)

The coordination mainly happens via regular calls which are scheduled in the [PM](https://github.com/ethereum/pm) repo. There are different kinds of developer calls with the biggest one being All Core Devs (ACD). This is where representatives of all involved teams come to discuss the current development of the consensus or execution layer.

其他的都看完了，在之前的部分。

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
