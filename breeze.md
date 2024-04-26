# breeze
I'm breeze, a Product Engineer specialized in JavaScript, Electron and automation scripts. Recently, I'm exploring web3. Excited to learn and contribute here!


## Notes
### 2024.4.26
继续POS相关机制；
相比pow的计算nounce，pos采用的是stake32个eth到contract中，然后运行三个软件，分别是an execution client, a consensus client, and a validator client.，成为validator；在每个epoch中会随机挑选一个validator来做验证:

1. 具体的工作流程如下：

注册：首先，想成为验证者的节点需要将32个ETH存入一个特定的智能合约中。

选中：并不是所有的验证者都会在每个时隙创建新的区块。相反，系统会在每个时隙（Slot）随机选择一个验证者来创建新的区块。这个过程是通过计算所有验证者的权益和第二步进队列的时间来完成的。

创建区块：选中后，验证者需要收集待处理的交易，将其包含在一个新的区块中。

广播：创建区块后，验证者需要将该区块广播到网络中的其他节点。

验证：其他的验证者需要接收这个新区块，验证其正确性，包括区块结构的有效性、交易的有效性，并重复执行区块中所有交易的状态转换确认其正确性。

批准：如果验证者确认新区块是有效的，那么他们会创建一个叫做"证明"（Attestation）的特殊消息，表示他们已经看到并确认了这个区块。

投票：接下来，验证者会基于他们的证明投票，决定哪个区块应该添加到区块链中。


2. 在上面的过程中，执行层，共识层，验证层的协作关系如下：

验证者客户端通过共识客户端来接收并传递网络中的信息。

当验证者被选为创建新区块的提议者时，执行客户端会处理交易并创建新区块，然后通过共识客户端发布到网络中。

当验证者需要验证新区块时，共识客户端会将新区块传递给执行客户端，执行客户端会验证新区块的所有交易，验证者客户
端则基于执行客户端的结果决定是否投票确认该区块。

### 2024.4.24
开启week3计划, 先刷pre-reading

eth consensus mechanisms
 - pow : 这里的原理和之前看到的bitcoin上的pow区别不大；比较大的区别是算法机制不同，以及奖励机制和出块时机机制不同
 - mining： [ethash](https://ethereum.org/en/developers/docs/consensus-mechanisms/pow/mining/mining-algorithms/ethash/)
 - pos


### 2024.4.23
刷完week2的视频；最后这一段提到了 p2p协议，以及snap相关；不过这里没有深入理解； 
不过顺着找到了这个仓库，看着不错，todo细研究：https://github.com/ethereumjs/ethereumjs-monorepo/tree/master
### 2024.4.21
刷视频：[Ethereum Execution Layer Overview | lightclient](https://www.youtube.com/watch?v=pniTkWo70OY)  还剩1/4没看完；

这个视频里面基本是通过代码演示整个执行过程，比起之前光看白皮书的理解更深了一点。

block building：

```go
func build(env Environment, pool txpool.Pool, state state.StateDB) (types.Block, state.StateDB, error) {
    var (
        gasUsed = 0
        txs []types.Transactions
    )
    for ; gasUsed < env.GasLimit || !pool.Empty(); {
        tx := pool.Pop()
        res, gas, err := vm.Run(env, tx, state)
        if err != nil {
            // tx invalid
            continue
        }
        gasUsed += gas
        txs = append(txs, tx)
        state = res
    }
    return core.Finalize(env, txs, state)
}
```
其中 进入到pool里面的tx已经排好了顺序，会优先处理支奖励高的；

block transaction：

- 走了一下go的eth的执行过程
- todo: 这里可以看一下类似用js实现的内部细节

多次提到了[EIP1559](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1559.md)，主要的改动是动态调整了base fee，而不是矿工指定；提供了更可预测的交易费用并改善网络拥塞




### 2024.4.20
week2视频： https://www.youtube.com/watch?v=7sxBjSfmROc

对应资料：可看https://cs251.stanford.edu/lectures/lecture7.pdf

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/25242467/e31e319f-987b-4995-a188-4f4f5d693339)

transaction types：
 - owned -> owned:  transfer ETH between users
 - owned -> contract: call contract with eth & data
 - contract -> contract
 - contract -> owned

在eth中的 transaction中包含nonce，这个和btc的nounce还不一样；具体区别如下

在以太坊（Ethereum）中：

Nonce主要用于防止单一账户的交易被重复发送。每一个新增的交易都会使得Nonce加一，因此相同Nonce的交易只会被网络接收一次，可以防止交易的重放。
Nonce还可以确保来自同一账户的多笔交易被按照发送的顺序执行。节点会按照Nonce的数值顺序来处理交易。

在比特币（Bitcoin）中：

Nonce是一个4字节的字段，其主要用途是作为工作量证明（Proof of Work， PoW）算法的一部分。Nonce值的更改使得哈希散列值的结果也会随之改变。
矿工在挖矿过程中，需不断改变Nonce值直至得出一个满足特定条件的哈希值，这个找到特定Nonce的过程需要极大计算能力和时间，体现了“工作量证明”。

视频里面介绍了域名系统的合约实现，类似ENS；

对应的原始代码如下，但是会有不少的漏洞；
``` solidity
function nameNew(bytes32 name) {
    // registration fee is 100 Wei
    if(data[name] == 0 && msg.value >= 100) {
         data[name].owner = msg.sender;  // record owner
         // log event (missing implementation)
    }
}
```
经过gpt4优化以后的代码

``` solidity 
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// 引入SafeMath库提供溢出保护
// import "openzeppelin-solidity/contracts/utils/math/SafeMath.sol";

contract NameSystem {
    // 使用SafeMath为所有uint运算提供额外的安全检查
    // using SafeMath for uint256;

    mapping(bytes32 => DomainDetails) public data;

    struct DomainDetails {
        bytes32 value;
        address owner;
    }

    event NameUpdated(bytes32 indexed name, bytes32 value, address indexed owner);

    // 修改后的nameUpdate函数
    function nameUpdate(bytes32 name, bytes32 newValue, address newOwner) public payable {
        // 使用require来确保条件是满足的，并且具有良好的错误提示
        require(msg.value >= 10, "Insufficient funds sent with transaction.");
        require(data[name].owner == msg.sender, "Caller is not the owner of the domain.");

        // 更新值和所有者
        data[name].value = newValue;
        data[name].owner = newOwner;

        // 触发事件日志
        emit NameUpdated(name, newValue, newOwner);

        // 退回超额支付的以太
        if(msg.value > 10) {
            payable(msg.sender).transfer(msg.value - 10);
        }
    }
    
    // 添加回退函数来处理无效的交易
    fallback() external payable {
        revert("Fallback function called.");
    }
}

```





### 2024.4.19

刷了week2里的Node and Client

1个node有两个client,  execution client and consensus client 

- 执行客户端：监听网络中广播的新交易，在EVM中执行它们，并持有所有当前以太坊数据的最新状态和数据库。
- 共识客户端：实施权益证明共识算法，使网络能够基于执行客户端的验证数据达成一致。

节点类型

- 全节点：逐块验证区块链，下载并验证每个块的区块体和状态数据。
- 归档节点：作为全节点，从创世块开始验证每个块，并且不删除任何已下载的数据。
- 轻节点：只下载区块头，需要的其他信息从全节点请求，可以独立验证接收到的数据。

同步模式
- 全归档同步：下载所有区块并逐步生成区块链状态。
- 快照同步：从最近的“可信”检查点开始，节省磁盘使用和网络带宽。
- 轻同步：下载所有区块头，部分区块数据，并进行随机验证。
- 乐观同步：允许执行节点通过已建立的方法进行同步。
- 检查点同步：基于弱主观性假设，从最近的弱主观性检查点开始同步。

现在整个社区有节点的运行方式有不同软件的实现，可以避免单点故障；

### 2024.4.18
- 做了题目，但是没全部刷完，到merge这一层的问题就不是很清楚了，得去刷一下相关的再回来重做
- 开始刷了weeke2视频 https://www.youtube.com/watch?v=pniTkWo70OY，开始涉及具体代码了

### 2024.4.16
继续白皮书，今天总体刷完；

针对货币供应相关：

- 以太坊网络采用了永久线性供应增长模型，而不是像比特币那样设定供应上限。尽管线性增长，但是随着时间的推移，以太坊的供应增长率同样趋于零。这是因为货币的丧失是无法避免的，这会使得实际的货币供应量最终稳定在年度发行量除以丧失率的值上。通胀率持续降低；

针对Light Node
- 以太坊的轻量级节点（Light Node）或轻客户端（Light Client）是一种特别设计的节点，相比全节点，只下载和验证与其相关的区块信息。
- 轻节点通过Merkle Patricia树数据结构确保下载的数据只是与其相关的部分，大大减少了存储空间和网络带宽需求。
- 轻节点适用于资源有限的环境，如移动设备或嵌入式系统。
- 轻节点在确认交易时需要依赖全节点。它只能验证其接收到的交易信息是否在区块链中，但不能验证区块链的完整性。
- 轻节点在以太坊生态系统中扮演着重要角色，为在运算能力和存储空间有限的设备上与区块链交互提供了一种方便、低成本的方式。

- 尽管轻节点具有一定的便捷性，但也存在安全风险，因为它需要依赖全节点来获取准确的区块链信息，可能容易受供应链攻击。

### 2024.4.15
继续eth白皮书：

Modified GHOST Implementation

在传统的区块链协议中，矿工总是选择最长的链来添加新的区块。然而，这种方法有一个问题，那就是它鼓励矿工只在最长链之上挖矿，而不考虑其他可能的分支。这导致许多已经挖掘出来的区块被浪费，因为他们不是在最长链上。

而GHOST协议提供了一种新的选择机制。它不仅考虑区块链的长度，还考虑了网络中未被包含在最长链中的区块的存在。也就是说，GHOST协议在选择最长链时，会考虑所有已知的区块，包括那些在短链上的区块。这样，即便是非最长链上的区块也有可能被纳入考虑，降低了区块的浪费，提高了区块链网络的安全性和效率。针对符合的uncle区块，对应的矿工也可以获得93.75%的奖励

针对fee的定价部分：

eth采用了gas价格+gas限额的方式；btc完全由矿工设置市场价格；

在图灵完备这个机制，为了解决无限循环调用相关问题，通过设置了最大执行步长以及执行需要损耗gas费用，如果最终gas费用不够，状态回滚，但是费用不会回滚；


在eth中，最小的基础单位为wei 其中  1 ether = 1e18 wei


### 2024.4.14
ETH应用部分：
- 纯金融应用：defi相关
- 半金融应用：涉及了金钱，但也包含了非货币层面，比如通过通过智能合约发放奖金和工资
- 非金融应用：不涉及货币，只是使用了纯去中心化能力，比如投票相关；

一些典型的应用：
- 代币系统
- 金融衍生品
- 身份识别：比如ens，注册唯一域名和地址帮忙；
- 去中心化文件系统
- 去中心化组织

其中去中心化组织里面，假设超过2/3的人同意，则要执行新的提案，但是正常合约部署以后是不可修改的，这时候需要实现合约可修改，可使用以下的方式：

设计思路：
- DAO是一段可自我修改的代码
- 如果有三分之二成员同意,代码就可以被更改
- 虽然代码理论上是不可改变的,但可以通过一些技巧实现事实上的可变性

具体做法：

- DAO的核心代码被分成若干部分,存储在不同的合约中
- DAO合约存储中保存了这些代码部分合约的地址
- 通过改变这些存储的地址值,即可实现指向新的代码合约,从而修改整个DAO的行为逻辑

DAO合约需要处理的三种交易类型:

- [0,i,K,V] - 提出编号为i的提案,将存储索引K处的值修改为V(一个新的合约地址)
- [1,i] - 投票支持编号为i的提案
- [2,i] - 如果提案i获得了足够的赞成票,执行该提案(修改索引K的值为V)

### 2024.4.13
继续白皮书，终于到eth的内容了；
总结了一下组成部分的要点

账户(Accounts):
- 外部拥有账户(Externally Owned Accounts):
  - 地址(20字节)
  - 余额
  - 发送交易时使用私钥签名

- 合约账户(Contract Accounts):
  - 地址(20字节)  
  - 余额
  - 合约代码
  - 存储(默认为空)

交易(Transactions):
- 接收者地址
- 发送者签名
- 转移的以太币金额
- 可选数据字段
- STARTGAS - 最大可用Gas
- GASPRICE - 每单位Gas支付的费用

消息(Messages):
- 发送者(隐式)
- 接收者
- 转移的以太币金额  
- 可选数据字段
- STARTGAS

gas费用：燃料费用，evm里面执行代码需要消耗的

eth区块链和挖矿：


![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/25242467/ad3b080a-6b58-4016-8e53-830dbfc26a70)

eth 和btc相比，它存储了完整的state的状态；
以太坊和比特币在存储区块状态时的主要区别在于:

比特币:
- 只存储最终的交易merkle树的根哈希
- 不单独存储每笔交易导致的状态变化
- 需要从创世区块开始重新执行所有交易才能重构整个UTXO状态

而以太坊:
- 存储整个状态Trie的结构,包括中间节点
- 单独存储了每笔交易导致状态Trie变化的部分
- 不需从创世重新执行,只根据状态根哈希及已存储的Trie节点就可以重构整个状态

以太坊的做法是将每笔交易导致的所有账户状态变化incrementally存储在Trie中,而不是像比特币那样只存储最终的哈希值。

这样做的好处是:
1) 更快重构状态,不需从头重放所有交易
2) 支持更复杂的状态和执行模型(如以太坊的智能合约)
3) 存储开销并没有想象中那么大,因为重复的状态只存储一次，采用了Patricia tree解决此类问题。实际存储空间相比比特币少了5-20倍；




### 2024.4.12
继续白皮书，其中一直提到了utxo，详细了解了一下， 

utxo：unspent Transaction output
在比特币系统中,UTXO 代表了可以作为未来交易输入的比特币金额。具体来说:

1. 每一笔比特币交易都会产生一个或多个输出,对应接收者可以使用的比特币金额。
2. 这些输出一旦被创建,就会进入 UTXO 池,表示为未使用的余额。
3. 后续的交易需要引用并使用 UTXO 池中的一些输出作为输入,即引用并"花费"前交易遗留的未花费余额。
4. 被引用使用后,这些 UTXO 将从 UTXO 池中移除,同时新的交易输出将被添加进 UTXO 池。
UTXO 实际上是比特币账户模型的实现方式。不同于记录账户余额,比特币网络跟踪并验证每个UTXO 的所有权和状态。用户的"余额"实际上是他们钱包控制下的所有 UTXO 的总和。

维护 UTXO 池的做法避免了需要处理整个交易历史进行余额计算,同时也确保了比特币不可被双重花费,因为每个 UTXO 只能被使用一次。这种设计使得比特币系统无需像传统账户模型那样进行全局状态同步。

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/25242467/e125ab75-684c-4289-835d-27f3e2dd8e77)


脚本语言在比特币链中会收到很多限制：
1. 缺少图灵完备；比如没办法使用循环；
2. 数据不可感知：UTXO脚本无法对输出的比特币金额进行细粒度控制和判断；UTXO是不可分割，要嘛完整使用要嘛不使用，很难精准到固定面额。这会导致无法实现金额相关的合约逻辑
3. 缺少状态，utxo只有两个状态， 已花费和未花费。而且每个utxo是独立的，无法组合，所以utxo智能支持简单，无状态的脚本；
4. 区块链数据不可感知；utxo无法访问链上数据，比如nounce和前块哈





### 2024.4.10
开始看 [ETH白皮书](https://ethereum.org/en/whitepaper/#ethereum-whitepaper)

ETH提供了一套图灵完备的区块链系统，你可以在上面通过智能合约来实现各种功能；

看了Introduction to Bitcoin and Existing Concepts Mining这一节；其中提到的双花攻击；
1. Send 100 BTC to a merchant in exchange for some product (preferably a rapid-delivery digital good)
2. Wait for the delivery of the product
3. Produce another transaction sending the same 100 BTC to himself
4. Try to convince the network that his transaction to himself was the one that came first.

如果要实现这个攻击，需要黑客在第一步之前的那个区块上再分叉出一个链出来，且需要超过51%的算力让这条分叉出来的链成为最长（因为区块链只认最长的那个节点），这样才能实现攻击；不过这个攻击会耗费巨大的资源，通常来说花费抵不上收益；另外历史上也出现过几次硬分叉的case，在eth链上，比较典型的是ETH和ETC.





### 2024.4.9
继续week1计划，先翻看了week1 的整体目录以及大致内容；后面计划针对白皮书和黄皮书，再细看；

[Design philosophy](https://web.archive.org/web/20220815014507mp_/https://ethereumbuilders.gitbooks.io/guide/content/en/design_philosophy.html)
- Simplicity
- Universality
- Modularity
- Non-Discrimination
- Agility

上面是eth的设计哲学，在其他领域其实同样适用；

eth两个重要的组成组件：
- Execution layer
- Consensus layer
其中执行层主要处理交易，共识层主要执行POS的的机制确保安全；



### 2024.4.8
继续前面的关于pow计算的问题, 之前对于nounce的值不清楚，这次结合bitcoin的白皮书+gpt4大概理了一下nounce计算的整个流程:

为了简化，我们将定义一个哈希函数，它只产生一个4位的数字，而且我们要求的PoW条件是这个数字要以“00”开头。因此，有效的哈希值可能是“0010”、“0078”、“0099”等，因为它们都是以“00”开头的。

第1步：准备数据
我们建立一个新的区块，其中包括待确认的交易数据，以及一些其他重要信息，比如前一个区块的哈希值。为了简单起见，假设这一块的数据是“blockdata1”，前一个区块的哈希是“abcd”。

第2步：进行哈希
为了找到有效的PoW，矿工开始尝试各种不同的nonce值，将其与区块数据合并，然后进行哈希。例如：

```
nonce = 1 -> 哈希("blockdata1abcd1") -> 1234 (无效，因为我们需要以“00”开头的哈希值)
nonce = 2 -> 哈希("blockdata1abcd2") -> 5678 (无效)
nonce = 3 -> 哈希("blockdata1abcd3") -> 9101 (无效)
...
```

矿工会继续尝试，直到找到一个有效的哈希值。

第3步：找到有效的PoW
假设当nonce值试验到“19”时，我们得到一个有效的哈希值：
```
nonce = 19 -> 哈希("blockdata1abcd19") -> 0023 (有效!)
```
这个“0023”就是一个有效的哈希值，因为它满足了我们的PoW条件：以“00”开头。 那么在实际的比特币区块链中，每个区块上对应的条件是在该区块生成的哈希值要小于或者等于目标哈希（区块上的Difficulty Target）；

第4步：广播并验证
矿工现在将这个包含有效nonce（19）的区块广播到网络中，其他节点接收到这个区块，会使用相同的哈希函数并验证：

第5步：链接到区块链
一旦区块通过网络接受，它就会被添加到区块链中，成为最新的区块，并将其哈希值用作后续区块的前一个哈希值的参考。

在实际的比特币网络中，哈希函数是SHA-256，输出结果有256位长，并且找到一个满足特定难度目标的nonce值是非常计算密集的。难度目标是根据当前网络的计算能力动态调整的，以确保平均每10分钟可以挖出一个区块。这是挖矿过程中寻找特定nonce值的基本概念。  另外nounce值不能精准计算得出，而是从0到x的一直反复尝试出来。这里建议结合一个具体的block的数据来进行推导；

### 2024.4.7
刷了视频: [But how does bitcoin actually work?
](https://www.youtube.com/watch?v=bBC-nXj3Ng4 )

- 从ledger到bitcoin chain；

- 使用sha256算法从a计算到b ，b很难反推出a

<img width="1147" alt="image" src="https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/25242467/82cda9c0-a6e3-4eb3-b573-db3fcc506d1f">

pow机制：接收响应，hash计算，创建区块，广播区块，接收奖励

视频里面讲了链上的pow机制，但是还是不清楚，所以刷了bitcoin的白皮书，目前还没看完，但是知道整个网络的运行过程大概如下：
1. 新交易的广播：一笔交易广播到网络中
2. 构建区块和寻找工作量证明Pow:矿工监听交易信息，从内存池里选择待确认的交易。
3. PoW的发现与区块广播：矿工计算符合当前链上难度的nounce值，如果计算出来，则将该区块广播出去。
4. 区块的接收&后续区块的创建


todo：
- 关于nounce的值是怎么计算得出；
- 目前minner是从内存池里面捞没有完成的交易出来，每个minner捞的交易内容是一样的吗，如果不一样的话，如果有重复的话，其中一个minner算得比较慢，那么没验证的交易是不是就要回到内存池里面；
- bitcoin的白皮书再细看一下，先了解清楚pow，在看后面eth的pos





### 2024.4.6




从week1开始，感觉内容开始很多了，有很多概念，因为之前看过eth的官方文档，所以有些大概还是知道的，所以目前策略是先刷完推荐的文章，然后针对感兴趣或者重点的再细看深入的部分。
1. 看了[v神的视频](https://www.youtube.com/watch?v=UihMqcj-cqc)， 记录了一些要点
- transaction：包含达成某个目的的信息
- accounts： eoa and contract
<img width="925" alt="image" src="https://github.com/weifengHuang/swift-dictionary/assets/25242467/9c1a304b-6e52-40b0-af00-c0aae3869f5b">

- gas: 消耗的基础单位
- proof of stake consensus：存储32个eth，成为一个validator； 验证者这有slasing机制，如果有欺骗类的操作，对应的stake eth会被没收， 达到costs of cheating are higher than the rewards. 
- fork choice： etc和eth之前是不是因为这个分叉过一次？什么场景使用
- casper ffg：超过2/3的validator统一某个区块的状态，则该区块达到finalized的状态, 这里之所以不是用的的1/2是为了增加系统的安全性，以及防止51%的攻击，而且有注意防止双花问题（网络节点被分成大小大致相等的两部分， 形成自己有效的区块链版本）
- layer2 

2. 刷了[The World Computer](https://inevitableeth.com/en/home/ethereum/world-computer) 这篇文章，这里面内容很多，整体看完了，但是每个章节里面还有一些深入的文章这个还没有细看，计划后面细看。


- The World Computer is made of up 3 parts: 
    - Ethereum Virtual Machine (EVM)
    - Ethereum Blockchain
    - Ethereum Network

- Scalability Trilemma，如下图所示，设计一个区块链网络需要同时考虑到安全性，去中心化，可扩展性。实现三者的最优是很难的。

<img width="836" alt="image" src="https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/25242467/0323fead-c9a9-4a63-b087-2bd1a48317d4">


明天的安排：
- 刷完视频： https://www.youtube.com/watch?v=bBC-nXj3Ng4 
- week1的文章继续深入
- 以及详细了解一下零信任；

### 2024.4.5
1. 了解了epf.wiki的计划，目前看下来不涉及到具体的开发，更多的是针对Eth底层协议的学习；
2. 对称加密和非对称加密过程和原理；以及可以关注一下[Signal](https://github.com/signalapp)的开源实现
3. Merkle trees 构建步骤：
 -  数据分块 首先,将需要验证的大量数据分割成等长的小块(例如512字节或1KB),并计算每个小块的哈希值。这层哈希值被称为"叶节点(Leaf Nodes)"。
- 构建中间层节点 将相邻的两个叶节点哈希值进行拼接(或直接连接),再对拼接后的值进行哈希运算。得到的新哈希值称为"父节点"。 例如,如果叶节点分别是H(0)和H(1)(H表示哈希函数),则父节点为H(H(0)||H(1))。
- 迭代哈希运算 重复第2步的过程,将相邻的父节点哈希值拼接并哈希,形成新的父节点,直至剩下一个根节点。这个根节点就是整个数据的"数字指纹"或"摘要"。
- 构建完整的Merkle树 最终,Merkle树是一个完整的二叉树,根节点表示整个数据集的摘要,叶节点表示原始数据块的哈希值,中间层节点表示相应数据块的组合摘要。

如图所示：
    ![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Hash_Tree.svg/2560px-Hash_Tree.svg.png)

使用这个数据结构的优点：
- 高效验证数据完整性:只需提供相应的部分叶节点哈希值和对应节点的路径,就可以重新计算出根节点哈希值,并与已知的根哈希值进行比对,从而验证数据的完整性。
- 节省存储空间:只需存储根哈希值和一些关键节点,而不需存储整个树,即可验证庞大数据集。
- 支持高效数据修改:修改部分数据只需重新计算涉及的节点,而不影响整个树的其他部分。

todo： 目前只是大概知道了节点里面的计算过程，但是整体在eth里面block是如何创建的还需要了解清楚；