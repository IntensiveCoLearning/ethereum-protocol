# breeze
I'm breeze, a Product Engineer specialized in JavaScript, Electron and automation scripts. Recently, I'm exploring web3. Excited to learn and contribute here!


## Notes

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