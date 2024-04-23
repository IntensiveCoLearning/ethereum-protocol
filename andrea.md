# andrea

区块链萌新

## Notes
### 2024.4.21
复习了一下拜占庭
![](https://inevitableeth.com/bft-5.jpeg)

[Casper FFG | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/casper-ffg)

Casper FFG is a process that exists atop a block proposal mechanism; it is responsible for finalizing these blocks, canonizing the true Ethereum blockchain.

Casper consists of two main parts: the finalization algorithm and the penalization scheme.

At its core, pBFT uses a voting scheme to provide mathematical certainty that a decentralized network has made a collective, final decision.

This is the principle that Casper applies: finalization certainty for Ethereum.

Rather than finalizing blocks, Casper finalizes "checkpoints," a single block once every N blocks (N = 32 blocks for PoS). The Casper equivalent of pBFT's voting is called a "supermajority link," meaning >2/3s of validators confirmed the validity of the checkpoint.

The two rounds of voting in pBFT are called prepare and commit; at the end of commit the action is final. In Casper, these ideas have different names: if a checkpoint has been confirmed once it is "justified." If it has been confirmed twice it is "finalized."

  
Every time a checkpoint is reached, each validator is responsible for evaluating the block and creating a vote. A vote simply marks the source checkpoint, the target block, proof of validity and the validator's signature.

Casper FFG is not a full BFT algorithm (hence "gadget"). It will guarantee safety, but liveness depends on the proposal mechanism.

However, Casper does provide new properties: 

- Accountability, violations of Casper can be detected and the violator can be identified 
- Defenses, against long range revision attacks and situations where >1/3 of nodes are offline 
- Dynamic, nodes can be added or removed

### 2024.4.20
继续DFS我前几天的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)
[State Transition Function | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/state-transition)

后面很多源码看不进去了🫠
![](https://inevitableeth.com/state-transition-7.jpeg)

![](https://inevitableeth.com/state-transition-8.png)



### 2024.4.19
继续DFS我前几天的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

[State Transition Function | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/state-transition)

![](https://inevitableeth.com/state-transition-3.jpeg)

#### Process slots
Sometimes a slot passes without a block being proposed; maybe the proposer was offline or the network dropped the block. The state transition function moves through empty slots and triggers a change of epoch, if needed. 

- Inputs: current state, slot 
- No output
![](https://inevitableeth.com/state-transition-5.jpeg)

While the current slot is less than the intended slot, progress forward (process_slot). 

If the next slot is going to be a new epoch, execute the processes associated with consensus and prepare for the next epoch (process_epoch).


### 2024.4.18
继续DFS我前几天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

[RANDAO | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/randao)
With BLS signatures, every validator already has a closely guarded random number - their private key - achieving unpredictability. Furthermore, every node can verify the RANDAO contribution just by verifying the BLS signature - achieving verifiability.

Specifically, the RANDAO contribution is its normal BLS signature with the the epoch number (think block number) as the message. 

This contribution is both stamped into the block (randao_reveal) and mixed into the EVMs RANDAO value.
![]()
![](https://inevitableeth.com/randao-3.png)

Each time a new block is created, RANDAO is updated with just a little more randomness. And so, through the trustless contribution of every proposing validator, we generate a sufficiently random value.

This value is now available to both Ethereum consensus and the EVM.

Practically speaking, if a dApp relies on true randomness, they are probably going to use a Verifiable Random Function (VRF) form an oracle like Chainlink. The true purpose of RANDAO is to provide randomness for consensus.

A protocol that is fully predictable is very vulnerable. An attacker could: 

- DDoS a proposer or a committee to attempt to halt the chain
- bribe an upcoming proposer
- attempt to register advantageous validator number to try to gain control over a committee - etc

### 2024.4.17
继续DFS我前几天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

[RANDAO | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/randao)

For example, a normal computer might use its users mouse movements as a basis of generating a random number. Mouse movements are very specific; it is very unlikely that two people will move their mice in the same way over a long enough period of time - intentionally or not.

Fortunately or not, the EVM is isolated from the outside world.

One way to solve this issue is with oracles, a type of service that bridge information between the World Computer and the internet. 

However, oracles are not native to Ethereum. 

Relying on an oracle has external trust assumptions and can be very gas intensive (expensive).

Instead, Ethereum relies on a RANDAO mechanism to create protocol-level randomness.

The RANDAO is a value maintained by the beacon chain; with each block, the proposer mixes in their own random contribution to the existing RANDAO value.

- **randao_reveal** - protocol-verified randomness, generated between all block proposers during an epoch.
    - Randomness is critical to the Beacon Chain; security depends on being able to unpredictably and uniformly select block proposers and committee members.
- **graffiti** - an (optional) 32-byte field in which block proposers can put anything they want. Often used by mining pools to log their blocks.
-  **signature** - the signature the block proposer creates to take responsibility (add to blockchain and collect reward if good, get slashed if bad).
    - Created by combining the BeaconState, BeaconBlock and the proposer's private key.
![](https://inevitableeth.com/block-consensus.jpeg)
-  **deposit_root** - the root hash of a Merkle tree which stores the ETH deposits into the staking contract (required to become a validator)
- **deposit_count** - amount of ETH in the staking contract
- **deposits** - [beaconcha.in](https://beaconcha.in/) defines this field as "amount of validator deposits which have been included in this block by the block proposer."
    - Interestingly, the only non-0 value I could find was in the genesis block
- **attestations** - a list of all the signatures that attested to this block
	Ethereum PoS elects a proposer who is charged with building (or selecting) a block and proposing it to the network. Attesters review the block and, if it's valid, sign it with their keys.
- **proposer_slashings** and **attester_slashings** - validators that have performed a hostile action against the network (for example, proposing or attesting to an invalid block). The network confiscates a portion of their staked ETH and ejects them from the validator set.
  
### 2024.4.16
继续DFS我前几天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

[Deep Dive: Time in Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/time)

After 32 slots, an epoch ends. 

At the end of every epoch, every validator runs process_epoch, a subroutine of the Ethereum State Transition Function. We'll summarize it in 2 sections: 

- Finalization 
- Consensus Housekeeping

#### Finalization

Finalization is the application of the Casper FFG protocol. Tl;dr Finalization is a promise by the protocol that an epoch (and therefore the blocks and the transactions within) are irreversible.

Finalization is the mathematical and economic guarantee that a specific action on the World Computer is part of the canonical blockchain. Undoing a single finalized transaction would necessitate destroying 1/3 of staked ETH - more than $20B, today.

Epochs mark the boundaries for finalization.

- If more than 2/3s of the network attest during an epoch, it becomes justified. 
- If a second epoch with a 2/3 majority follows the first, it will finalize that epoch, granting it the security guarantees of Ethereum

#### Consensus Housekeeping

The other section of process_epoch is consensus and housekeeping. Basically this is everything needed to uphold the rules of consensus (processing slashing, rewards, etc) and reseting the stage for the next epoch. 

Read up on Casper FFG for more info.

Both are considered mathematically safe, but DSA is faster at decrypting and signing, while RSA is faster at encrypting and verifying. The differences appear (mostly) in the encrypt, decrypt and verification function.

[BLS Digital Signatures | Inevitable Ethereum](https://inevitableeth.com/home/concepts/bls-signatures)

[Boneh–Lynn–Shacham Digital Signatures (BLS signatures)](https://en.wikipedia.org/wiki/BLS_digital_signature) are a specific type of digital signature. BLS signatures function perfectly fine (albeit relatively slowly) as standard cryptographic scheme, but the real magic comes from aggregation.
  
Aggregation means that given a single message, multiple signatures can be verified with a single operation.

Since aggregate signatures are indistinguishable from normal signatures, and aggregate public keys are indistinguishable from normal public keys, we can reuse our normal verification algorithm.

Thus, a single operation can verify a huge amount of signatures.

As previously mentioned, BLS signatures are computationally expensive when compared to verifying a more standard scheme - more than an order of magnitude slower. However, each verification can count for MUCH more than a standard scheme (a single verification).

Imagine you need to verify 100 signatures: 

- Standard digital signature: x time/verification * 100 verifications = 100x 
- BLS digital signature: 10x time/verification * 1 verification = 10x 

The more signatures you can aggregate, the higher the savings

But verification speed is not the only benefit: BLS signatures offer huge space savings over non-aggregated signatures. An aggregated signatures is the same size as a single signature, regardless of how many signatures have been aggregated.

Imagine you need to verify 100 signatures: 

- Standard digital signature: x bytes/signature * 100 signatures = 100x 
- BLS digital signature: x bytes/signature * 1 signature = x 

More aggregation equals more savings, but even more with space than speed.

BLS signatures are a type of digital signature that provide the same guarantees as any signature (authenticity and liveness) but provide huge scaling benefits when verifying large groups of signatures.
### 2024.4.15
继续DFS我前几天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)
[Deep Dive: Time in Ethereum](https://inevitableeth.com/home/ethereum/network/consensus/time)

Every 12 seconds, Ethereum opens a new slot, expecting a new block. 

Within a block there are thousands [transactions](https://inevitableeth.com/home/ethereum/blockchain/block), but they execute atomically: either all together or none at all.

An epoch is made up of 32 slots.

A [BLS signature](https://inevitableeth.com/home/concepts/bls-signatures) is a [digital signature](https://inevitableeth.com/home/concepts/digital-signatures) that provides all the normal guarantees (proof a specific message was signed by a specific person) but has a useful bonus property: it can be aggregated. 

Once aggregated, thousands of signatures can be verified in one operation.

For those of you not following along at home, 440k validators / 64 committees = 27k validators / committees. 

This is too big a number to aggregate all at once. 

And so, committees are broken up into 128 subnets. ~200 validators / subnet

In each subnet, 16 validators are designated as aggregators. All subnet-members publish their BLS signatures, but only aggregators listen and do the aggregation. All 16 are trying to build the same ideal aggregate signature, but conditions are often not ideal.

Next, the block proposer will pick the best BLS aggregate signature, one from each of the 128 subnets The BLS aggregation algorithm is applied one final time, and the 128 subnet signatures are merged into one final committee BLS signature, representing ~27k validators

As an aside, this whole process is the reason 32 ETH is the minimum amount of ETH required to become a validator. This aggregation process is slow and complex; reducing the minimum stake increases the number of validators, exponentially increasing the problem.

### 2024.4.14
了解了一些SSZ编码

[Simple serialize | ethereum.org](https://ethereum.org/en/developers/docs/data-structures-and-encoding/ssz/)

So the actual values for variable-length types are stored in a heap at the end of the serialized object with their offsets stored in their correct positions in the ordered list of fields.

There are also some special cases that require specific treatment, such as the `BitList` type that requires a length cap to be added during serialization and removed during deserialization. Full details are available in the [SSZ spec(opens in a new tab)](https://github.com/ethereum/consensus-specs/blob/dev/ssz/simple-serialize.md).

There are also cases where the leaves of the tree do not naturally evenly distribute in the way they do in the example above. For example, leaf 4 could be a container with multiple elements that require additional "depth" to be added to the Merkle tree, creating an uneven tree.

[ssz.dev/overview](https://www.ssz.dev/overview) 交互式表现
### 2024.4.13
深入了解了一下RLP编码和EIP-2718

[以太坊RLP(递归长度前缀)编码 | 登链社区 | 区块链技术社区 (learnblockchain.cn)](https://learnblockchain.cn/2019/05/20/geth-rlp-encode/)

[以太坊上新的事务类型：EIP-2718 简介 | 登链社区 | 区块链技术社区 (learnblockchain.cn)](https://learnblockchain.cn/article/2528)

### 2024.4.12
DFS我前几天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

[Ethereum Transaction | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/blockchain/transaction)

An Ethereum transaction is made of up 3 parts:

- metadata, including to/from, $ETH amount, gas details and signature data
- cache, a list of accounts and keys the transaction expects to use
- data, the payload of the transaction (smart contract code or API call)

![](https://inevitableeth.com/transaction-full-txn.jpeg)

Cache contains the accessList, a list of addresses and keys the transaction anticipates using. The transaction will still be able to use resources off this list, but at a higher (gas)cost.

The accessList was added by EIP-2929, allowing clients to fetch/cache data to be used during the transaction. 

Today, the discount for using addresses & keys in the accessList is ~10%. However, this will increase in the future as Ethereum moves to support light clients.

The data payload being delivered by the transaction. This can be used in 3 ways:

- ETH transfer - empty
- smart contract API call - name of function and parameters
- new smart contract - code of the smart contract

Data in the input field is recorded in binary, but can be translated back to a human readable form.

The input field exists on-chain, but is not part of the EVM state. It simply provides data for the contract to use during the transaction, it is not tracked by Ethereum nor used in consensus.

The EVM can only use data supplied in that transaction; it cannot look back.

This property becomes useful for applications that want to write historical data to the Ethereum blockchain (eg for manual retrieval later) but don't care about having direct EVM access.

[Ethereum Gas | Inevitable Ethereum](https://inevitableeth.com/home/concepts/gas)

The EVM has computational costs; every activity that touches the World Computer can be boiled down to the machine code readable by the EVM. That bytecode is made of operations that each have a specific gas cost. 

Therefore, gas measures the amount of computational effort required to execute specific operations.

This gas is consumed by the World Computer in the same way that electricity is consumed by a (physical) computer; it is gone forever. Ethereum does this by "burning" the gas, or sending it to a permanently unrecoverable address.

In order to actually run a transaction, you must supply the World Computer with enough gas to execute it.

一个对Gas非常好的理解

Gas is complicated, there are 2 markets you need to follow when making a transaction: 

- ETH (priced in $) 
- gas (priced in ETH/[GWEI](https://www.investopedia.com/terms/g/gwei-ethereum.asp)). 

Think of it this way: does you computer care about the cost of electricity? 

Why would the World Computer care about the cost of ETH?

Gas is an abstraction that allows us to have a distinct value layer for computational expenses vs the valuation of ETH. 

The World Computer is a globally shared, scare resource. Gas is how we divide up the units of the EVM, and then we let the market distribute it.

ETH(Gas) 就像水电，而水电目前需要用法币来支付

Finally, gas fees keeps Ethereum secure. 

By requiring a fee for every transaction, [spam attacks](https://en.wikipedia.org/wiki/Denial-of-service_attack) quickly become nonviable. Infinite loops or other computational wastage quickly burn themselves out.

And higher gas fees = more $ETH burn = higher % staked = more economic security.


### 2024.4.9
DFS我前几天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

[Ethereum Node | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/network/node)

还看了很多 比较简单 就没有放笔记 基本就是组合起来之前已经知道的知识。
### 2024.4.9
合约元数据文档：[Contract Metadata — Solidity 0.5.2 documentation (soliditylang.org)](https://docs.soliditylang.org/en/v0.5.2/metadata.html)

合约反编译器：[Online Solidity Decompiler (ethervm.io)](https://ethervm.io/decompile)

 A standard transaction, from address A to address B with no contracts involved, will use a fixed amount of 21,000 gas.
The gas price is commonly denoted in gwei and is on a scale that starts from 0 and goes to 500+ in some extreme cases.

今天看了以太坊最新的黄皮书：[paper.pdf (ethereum.github.io)](https://ethereum.github.io/yellowpaper/paper.pdf)

Throughout the present work, any reference to value, in the context of Ether, currency, a balance or a payment, should be assumed to be counted in Wei.

Occasionally actors do not agree on a protocol change, and a permanent fork occurs. In order to distinguish be tween diverged blockchains, EIP-155 by Buterin [2016] introduced the concept of chain ID, which we denote by $\beta$ . For the Ethereum main network $\beta=1$

The trie requires a simple database backend that maintains a map ping of byte arrays to byte arrays; we name this underlying database the state database. This has a number of benefits; firstly the root node of this structure is cryptographically dependent on all internal data and as such its hash can be used as a secure identity for the entire system state. Secondly, being an immutable data structure, it allows any previous state (whose root hash is known) to be recalled by simply altering the root hash accordingly. Since we store all such root hashes in the blockchain, we are able to trivially revert to old states.

下次再说吧 不是我能看懂的 🫠 
### 2024.4.8
继续我昨天看的文章[The Ethereum Virtual Machine — How does it work? | by Luit Hollander | MyCrypto | Medium](https://medium.com/mycrypto/the-ethereum-virtual-machine-how-does-it-work-9abac2b7c9e)

the opcode **KECCAK256** (formerly known as **SHA3**) has a base cost of **30 gas**, and a dynamic cost of **6 gas per word** (words are 256-bit items). Computationally expensive instructions charge a higher gas fee than simple, straightforward instructions. On top of that, every transaction starts at **21000 gas**.

When executing instructions which reduce state size, gas can also be refunded. Setting a storage value to zero from non-zero refunds **15000 gas**, while completely removing a contract (using the **SELFDESTRUCT** opcode) refunds **24000 gas**. Refunds only occur after contract execution has completed, thus contracts cannot pay for themselves. Additionally, a refund cannot exceed half the gas used for the current contract call.

When deploying a smart contract, a regular transaction is created, without a `to` address. Additionally, some bytecode is added as input data. This bytecode acts as a **_constructor_**, which is needed to write initial variables to storage before copying the **_runtime bytecode_** to the contract’s code. During deployment, creation bytecode will only run **once**, while runtime bytecode will run **on every contract call**.

At the end of this bytecode, a [**Swarm hash**](https://github.com/ethereum/wiki/wiki/Swarm-Hash) of a [**metadata file**](https://solidity.readthedocs.io/en/v0.5.2/metadata.html) created by Solidity gets appended. Swarm is a distributed storage platform and content distribution service, or, more simply stated: a decentralized file storage. Although the Swarm hash will also be included in the runtime bytecode, it will never be interpreted as opcodes by the EVM, because its location can never be reached. Currently, Solidity utilizes the following format:

`0xa1 0x65 'b' 'z' 'z' 'r' '0' 0x58 0x20 [32 bytes swarm hash] 0x00 0x29`

Therefore, in this case, we can extract the following Swarm hash:

`4e048d6cab20eb0d9f95671510277b55a61a582250e04db7f6587a1bebc134d2`  
  
The metadata file contains various information about the contract, such as the compiler version or the contract’s functions. Unfortunately, this is an experimental feature, and not many contracts have publicly uploaded their metadata to the Swarm network.

Several projects have created tools to attempt making bytecode more readable. For example, you can try decompiling a contract on mainnet using [**eveem.org**](https://eveem.org/) or [**ethervm.io**](https://ethervm.io/). Unfortunately, some parts of the original contract source, such as functions names or event names, are always lost due to optimization done by the compiler. Nonetheless, most function names can still be brute forced by comparing function signatures to large datasets containing popular function and event names (see [**4byte.directory**](https://www.4byte.directory/)).

Contract calls usually require an “**ABI**” (**_A_**_pplication_ **_B_**_inary_ **_I_**_nterface_), which is a piece of data documenting all functions and events, including their needed input and output. When calling a function on a contract, the _function signature_ is determined by hashing the name of the function including its inputs (using [keccak256](https://en.wikipedia.org/wiki/SHA-3)), and truncating everything but the first **4 bytes**.

As you can see in the image above, our function `HelloWorld()` resolves to the signature hash `0x7fffb7bd`. If we would like to call this function, our transaction data needs to start with 0x7fffb7bd. Arguments which need to be passed to a function (none in this case) can be added in **32-byte pieces** called **words** after the signature hash in a transaction’s input data.

If an argument contains over 32 bytes (256 bits) of data, like an array or string, the argument is split into multiple words which are added to the input data after all other arguments have been included. Moreover, the total size of all words gets included as another word, before all array words. At the location where the argument would have been included, the start position of the array words (including the size word) is added instead.
### 2024.4.7
DFS我昨天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

counter-party risk

[Ethereum Virtual Machine | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/evm) Resources -- Further reading
#### Downsides of the Ethereum Virtual Machine
•   **The EVM network isn’t entirely decentralized.** The vast majority of Ethereum nodes are hosted on centralized cloud servers like Amazon Web Services. If the owners of such services decide they don’t like Ethereum for some reason, the nodes could easily be shut down, damaging or destroying the network. This has happened before with certain social media apps, for example.

•   **The EVM requires some technical knowledge.** Those who don’t know how to code can’t do much with the EVM. More user-friendly interfaces are still in the process of being developed. NFTs are a good example again — there are programs that have graphical user interfaces (GUIs) that allow almost anyone to create NFTs and use related marketplaces.

•   **High gas fees during times of network congestion.** This can be a big downside for users of Ethereum. While those sending large transactions might not be affected as much, everyone trying to send smaller transactions might be unable to use the network for a time. In particular, this creates problems for decentralized applications. When a lot of users are interacting with the DApps’ smart contracts and creating many transactions, things can slow down to a crawl or even stop working when gas fees get too high.

From [What Is the Ethereum Virtual Machine (EVM)? | SoFi](https://www.sofi.com/learn/content/what-is-ethereum-virtual-machine/)

------

EVM 操作码：死去的汇编记忆又复活了 🫠
- `CALLER` 获取调用者的地址
  
- `CALLVALUE` 获取随调用（交易）发送的 eth 数量

- `NUMBER` 获取当前区块的编号
- `LT`（Less Than）：此操作码用于比较栈顶的两个元素。如果第一个元素小于第二个元素，则将 1（真）压入栈顶；否则，压入 0（假）。
- `GT`（Greater Than）：此操作码类似于 `LT`，但用于检查第一个元素是否大于第二个元素。如果第一个元素大于第二个元素，则将 1（真）压入栈顶；否则，压入 0（假）。
- `MLOAD` (Memory Load): 这个操作码用于从内存中读取数据。它接受一个参数，即内存中的起始地址，并从该地址读取一个完整的字（32字节）的数据，然后将其压入栈顶。这用于从内存中检索存储的数据。
- `MSTORE` (Memory Store): `MSTORE`操作码用于将数据写入内存。它接受两个参数：第一个是内存中的起始地址，第二个是要存储的值。这个操作将栈顶的值（一个完整的字，即32字节）存储到指定的内存地址。这是智能合约在内存中存储数据的主要方式。
- `MSTORE8`: 这个操作码与`MSTORE`类似，但它只存储一个字节的数据而不是一个完整的字。这对于存储小量数据时非常有用，因为它允许更精细的内存使用。
- `MSIZE` (Memory Size): `MSIZE`操作码用于获取当前内存的大小。内存大小是动态的，随着`MSTORE`或`MSTORE8`操作的使用而增长。`MSIZE`返回的大小是当前已分配内存的字节数。
- `SLOAD` (Storage Load)：`SLOAD` 操作码用于从智能合约的存储中读取数据。它接受一个参数，即存储位置的键（通常是一个 32 字节的数值），并从该位置检索存储的值（也是一个 32 字节的数值），然后将这个值压入栈顶。这用于从合约存储中获取持久数据。
- `SSTORE` (Storage Store)：`SSTORE` 操作码用于将数据写入智能合约的存储。它接受两个参数：第一个是存储位置的键，第二个是要存储的值。这个操作将栈顶的值存储到指定的存储位置。`SSTORE` 用于修改或设置合约存储中的数据。
	PS: 存储操作对于智能合约非常重要，因为它们允许合约保持跨交易的状态。与内存（由 `MLOAD` 和 `MSTORE` 操作处理）不同，存储在区块链上永久保存，即使在合约调用结束后也会保留。
- `JUMP`: 这个操作码用于无条件跳转。它从栈顶弹出一个地址，并将程序计数器（PC）跳转到那个地址。这相当于其他编程语言中的“goto”语句。它使得合约能够跳转到代码中的不同部分，但出于安全原因，跳转只能发生在被标记为 `JUMPDEST` 的位置。
- `JUMPI`: `JUMPI` 是一个条件跳转操作码。它从栈中弹出两个值：第一个是目标地址，第二个是条件。如果条件非零，则执行跳转到指定的地址；否则，继续按顺序执行。这类似于其他语言中的“if”语句或“conditional jump”。
- `PC` (Program Counter): `PC` 操作码将当前的程序计数器值压入栈顶。程序计数器是一个内部指针，指向当前正在执行的指令。这可以用于实现更复杂的控制结构，或用于调试目的。
- `JUMPDEST`: `JUMPDEST` 标记一个合法的跳转目标。在代码中，`JUMPDEST` 操作码用来指示一个位置，`JUMP` 或 `JUMPI` 可以安全地跳转到这个位置。这是为了防止通过跳转进入数据段或中途进入操作码序列，可能导致未定义行为。
- `STOP`: 这个操作码用于停止执行并退出当前的合约调用。`STOP` 用于正常结束一个合约的执行，没有任何错误或异常。在 `STOP` 之后，不会再执行更多的代码，并且合约的状态更改被保留。
- `RETURN`: `RETURN` 用于停止执行，返回数据给调用者。这个操作码通常在合约函数想要返回数据时使用。它从栈中获取两个参数：数据的起始位置和数据长度，然后将这些数据返回给调用者。与 `STOP` 一样，合约状态更改在使用 `RETURN` 之后被保留。
- `REVERT`: `REVERT` 用于撤销执行，撤销所有状态更改，并返回错误信息或其它数据给调用者。这个操作码在合约执行出错或需要撤销执行时使用，例如，当满足某些条件时中断执行。使用 `REVERT` 可以保证不消耗所有提供的Gas。
- `INVALID`: `INVALID` 操作码用于表示非法操作。这通常用于错误处理。当遇到 `INVALID` 操作码时，执行将停止，所有的状态更改将被撤销，且所有剩余的Gas将被消耗。这通常表示字节码级别的错误或意外情况。
- `SELFDESTRUCT`: `SELFDESTRUCT` 用于销毁智能合约，并将合约中剩余的以太币发送到指定地址。这个操作码不仅停止执行，还从以太坊区块链上移除合约代码和存储，这有助于节省空间。`SELFDESTRUCT` 通常用于升级合约或结束合约生命周期。


![](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*I4v8ArsePBK_iFSxgljxTg.png)

However, writing to storage is very expensive (**as much as 6000x**) compared to writing to memory.

From [The Ethereum Virtual Machine — How does it work? | by Luit Hollander | MyCrypto | Medium](https://medium.com/mycrypto/the-ethereum-virtual-machine-how-does-it-work-9abac2b7c9e)

### 2024.4.5

DFS我昨天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)
#### [Credible Neutrality | Inevitable Ethereum](https://inevitableeth.com/home/concepts/credible-neutrality)
##### [Vitalik's Principle](https://nakamoto.com/credible-neutrality/)

When building mechanisms that decide high-stakes outcomes, it’s very important for those mechanisms to be credibly neutral.

A mechanism is a tool that takes in inputs from multiple people, and uses these inputs to make some kind of decision that people care about in a way that is consistent with its participants’ values.

A mechanism is an algorithm plus incentives.

![](https://inevitableeth.com/credible-neutrality-chart.jpeg)

Mechanisms such as blockchains, political systems and social media are designed to facilitate cooperation across large, and diverse, groups of people. 

Credible neutrality is about both bringing a new participant to a mechanism and keeping its existing users. 

- Neutrality is about everyone seeing that the mechanism is fair.
- Credible is about everyone seeing that everyone can see that as well.

Everyone participating wants to be sure that everyone else will not abandon the mechanism the next day.
### 2024.4.4
[Week 1 (epf.wiki)](https://epf.wiki/?#/eps/week1)

[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)

从里面看到了一些我感兴趣的东西，研究了一下
#### [State Size Management Theory | Inevitable Ethereum](https://inevitableeth.com/home/ethereum/upgrades/statelessness/theory)

A user only has to pay once to increase the state size, but the increased size will create a perpetual burden (cost) on the network.

Instead of allowing the state of the EVM to grow into infinity, we could look at schemes that move inactive parts of the state into retirement. 

State Expiry schemes do not look to delete data out of the EVM state; any inactive nodes can be cryptographically resurrected.

The first question when considering a state expiry scheme is how to renew state. 

While there are many ideas, the community is moving towards a model based on "touching" the data. In general, the idea is all data is set to expire by default and must be explicitly renewed.

Data that expires can be revived. 

A resurrection would require a proof that shows the data is currently part of the inactive state. 

Proof generation would require being able to access the inactive state, locally or otherwise (think Etherscan or Alchemy).

From here, state expiry gets much more configurable (and complicated). Some of the more important toggles: 

- Expiry at the account or storage-slot level 
- Creating an inactive state tree or just marking nodes as inactive 
- Managing resurrection conflicts

At the end of the spectrum is Strong Statelessness, the idea that no node (or block producer) needs to hold the state. 

Every participant can be 100% stateless.

Instead of using a local copy of the EVM state to validate transactions, transaction senders would provide proofs that guarantee the validity of their transactions. 

Transaction senders (or, realistically, 3rd parties) would be responsible for storing enough of the state tree to generate proofs.

This dynamic highlights a core property of state size management: there are many options creating complicated trade-offs. 

For example, strong statelessness moving so much burden from the network to those using it.

Taking a step back, any move towards statelessness increases the amount of data that moves through the network (bandwidth requirements). 

Proofs are lightweight, but every transaction must be accompanied by 1+ proofs (components).

And so, in conclusion, the solutions to Ethereum state size management are a spectrum. There is no "right" solution, there are only decisions to be made.


### 2024.4.3

https://epf.wiki/?#/eps/week0?id=helpful-resources-to-get-you-started

标注一些我以前还不知道的东西：

来源：[53-BEIKO-001-2023-12-13.pdf (summerofprotocols.com)](https://summerofprotocols.com/wp-content/uploads/2023/12/53-BEIKO-001-2023-12-13.pdf)

A more recent example of practical secure
communication in today’s chat era is
the Signal protocol. Implemented in the
homonymous application, the Signal
protocol uses a form of public keys that
allow a seamless user experience for both
one-to-one and many-to-many (group
chat) conversations. The use of the Signal
protocol became the golden standard of
encrypted low-latency communication,
gaining adoption from other major
applications such as WhatsApp.

[What is BitTorrent? (youtube.com)](https://www.youtube.com/watch?v=xH00ikD1oDo)

分散upload带宽，增加下载速度
