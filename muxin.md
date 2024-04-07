# Muxin

Hello guys, I'm Muxin, I'm learning everything about Ethereum, especially for Ethereum Protocol. I'm good at Web development. Follow me on Twitter: <https://twitter.com/muxin_eth>, my telegram: <https://t.me/muxin_eth>.

## Notes

### 2024.4.5

今天学习了 Week 0 Pre-reading 的部分

**1. Cryptography**

之前没有系统的了解过，所以这次稍微花了点时间

密码学分为古典密码学和现代密码学：

- 古典密码学：主要关注信息的保密书写和传递，以及与其对应的破译方法，比较依赖于设计者和敌手的创造力与技巧，作为一种实用性艺术存在。简单来说加密就是把普通信息转换成难以理解的信息，解密就是相反的过程。比较常用在战争、宗教（加密观点，免遭迫害）、“情书”等。
  - Caesar cipher（凯撒密码）：使用的是替换法，每个字母被往后位移三个字母所替代。
  - Vigenère cipher（维吉尼亚密码）：使用的是多字符加密法，加密重复使用到一个关键词，用哪个字母取代端视轮替到关键字的哪个字母而定。
  - Enigma machine（恩尼格玛密码机）：是德国在第二次世界大战中的重要工具。
- 现代密码学：除了关注信息保密问题外，还涉及信息验证码、数字签名、分布式计算中产生的信息安全问题。
  - symmetric-key encryption（对称密钥加密）：消息被一个密钥 key 加密，消息发送者会把密钥 key 和加密后的消息发送给接收者，只要有密钥 key 就能将对应的加密消息解密。但问题是需要保证密钥 key 的安全性。
  - asymmetric encryption（非对称加密）：有一对加密密钥（公钥-由私钥按着一定规则生成，a large number）和解密密钥（私钥- a large and random number），用加密密钥加密后的消息只能通过解密密钥来解密，所以必须要知道两个密钥才能解密。
  - digital signature（数字签名）：类似于手写签名，使用了公钥加密技术，签名者生成一对密钥（公钥和私钥），私钥用于生成数字签名，公钥用于验证签名的真实性。数字签名具有完整性、认证性和不可否认性。
  - signal protocol（加密通信协议）：由 Signal Messenger 公司开发，被广泛用于各种即时通讯应用中，如 Signal、WhatsApp 和 Facebook Messenger。它提供了端到端加密，确保消息只能被发送方和接收方阅读，不会被中间人窃取和篡改。

Hashing：
Hash 是将任意长度的输入数据通过哈希函数转换成固定长度的输出数据的过程，对于不同的输入数据，哈希函数应该生成不同的哈希值，即使输入数据变化很小，生成的哈希值也会有大幅度变化，且哈希函数是单向的，你无法通过哈希值还原出原始输入数据。

**2. Merkle tree in Bitcoin**

在区块链中，每一个区块头部都包含了一个称为 Merkle 根的哈希值，它是由区块中所有交易数据构建的 Merkle 树的根节点的哈希值。在形成一个新的区块时，交易数据会被组织成 Merkle 树。首先，对每个交易数据进行单独的哈希运算得到叶子节点。然后，依次将相邻的叶子节点两两配对，并计算它们的哈希值。如此反复，直到只剩下一个根节点，即 Merkle 根。

当其他节点接收到新的区块时，它们可以通过比对区块头部中的 Merkle 根与交易数据中的哈希值来验证交易数据的完整性。只需获取到少量的数据和 Merkle 树的根节点，节点即可通过递归地比对哈希值验证整个区块中的交易数据是否被篡改。它只需要通过比对树的根节点来验证整个区块中的交易数据，而不需要获取整个区块的数据，所以提升了验证交易数据的高效性。

![merkle tree](./image-2.png)

**3. P2P network**

点对点的分布式网络，没有中心化服务器，每个节点都是对等的，可以互相通信、交换资源和共享信息。在以太坊中主要应用在：区块传播、交易传播、状态同步、共识算法、发现节点等。

**4. Bittorrent**

一种用于文件共享的协议，它是一个基于 P2P network 的协议，旨在实现高效的大规模文件分发。简单来说，它使用分布式的方式进行文件传输，文件被划分成小块，这些小块由不同的节点共享和下载，提高了下载效率。

（文件的共享者会创建一个种子文件（.torrent ），它包含了文件的元数据信息、哈希值以及 tracker 地址等，每个节点都通过下载种子文件来获取文件的信息。Tracker 是一个服务器，用于协调各个节点之间的连接和数据传输。当用户想要下载某个文件时，他们的 Bittorrent 客户端会向 Tracker 发送请求，获取其他拥有相同文件的节点的信息。）

### 2024.4.6

今天学习的是 Week 1 的内容

> Week 1 pre-reading 的这部分 https://inevitableeth.com/home/ethereum/world-computer 之前有看过，感觉里面介绍的每一个概念都需要好好深入学习，里面针对每一个概念也都有对应的 Deep Dive 的链接，我是想先尝试学习 protocol 的部分，如果涉及到了不懂的地方再回头补上。

没有看视频，学习内容来源：

- https://epf.wiki/#/eps/week1?id=study-group-week-1-protocol-intro
- https://twitter.com/EIPFun/status/1759938839890522603 感谢 Chloe 的整理

**1. Prehistory and Philosophy**

这里提到了 UNIX，它是一个重新定义了计算范式的操作系统和哲学。这个范式已经使用了 50 多年，从未真正改变过。其模块化的基本概念对以太坊的设计和贝尔实验室的开放协作环境有重要意义。可以看一下这个视频（from Dennis Ritchie and Ken Thompson）：https://yewtu.be/watch?v=tc4ROCJYbm0。

这里还提到了自由软件运动是以太坊和所有加密货币的基础。以太坊的开放、独立和协作开发文化深深植根于 FOSS（Free and Open Source Software），以太坊需要在软件中透明地实现，为用户提供充分的自由。扩展阅读在后面，需要了解一下什么是 free software，以及它的重要性。

非对称密码学的发明标志着密码学应用新范式的诞生。

**2. Ethereum Protocol Design**

目前的协议规范是由 Python 实现的，包括了：Exection specs 和 Consensus specs，并且会在 EIP 社区中进行变更的跟踪。每次网络升级也都会进行协议的更新，但是总会遵循这几个原则：Simplicity, Universality, Modularity, Non-discrimination, Agility。

**3. Implementations and Development**

执行层（EL）和共识层（CL）的实现称为客户端，运行此客户端并连接到网络的计算机称为节点。并且以太坊是允许使用不用语言来实现，经过时间的洗礼，有些也已经废弃了，这里是列表https://ethereum.org/en/developers/docs/nodes-and-clients/#execution-clients。有关 EL 和 CL 会在之后进行详细的学习。

![exection-clients-language](./image-3.png)

**4. Testing**

由于有定期的变更和多个客户端的实现，测试对于 network 安全来说是非常重要的，根据不同的场景/客户端也会有不同的测试工具。 包括 state transition testing, fuzzing, shadow forks, RPC tests, client unit tests and CI/CD 等。

**5. Coordination**

以太坊不像中心化的公司，它是完全去中心化、公开的组织，任何人都可以参与贡献，每个人都可以参与到以太坊中自己最感兴趣的部分，其中开发的（新的 feature 或 changes）流程是：idea - research - development - testing - adoption，如果大家有问题、idea、以及获取最新的 update，都可以去社区内定期的会议（ACD，可以在 https://github.com/ethereum/pm 查看 schedule 和 notes）、discord（https://discord.com/invite/qGpsxSA）、forum（https://twitter.com/EthMagicians，https://twitter.com/ethresearchbot）等。

里面提到的一些比较感兴趣需要额外学习的文档或书籍（TODO）：

- https://www.deusinmachina.net/p/history-of-unix
- https://yewtu.be/watch?v=Ag1AKIl_2GM
- https://www.gnu.org/philosophy/free-sw.html
- https://ethereum.org/en/whitepaper/#ethereum-whitepaper
- https://ethereum.github.io/yellowpaper/paper.pdf
- https://ethroadmap.com/
- https://archive.devcon.org/archive/
- https://www.camirusso.com/book 这本书看过中文翻译版本，翻译的比较混乱，有时间可以在看一下原版
- https://www.goodreads.com/book/show/55360267-out-of-the-ether
- https://www.routledge.com/Absolute-Essentials-of-Ethereum/Dylan-Ennis/p/book/9781032334189
- https://github.com/ethereumbook/ethereumbook
- 还有一个 Ethereum quiz，明天可以做一下

### 2024.4.7

今天做了一下以太坊官网上的 quizzes，可以很明显的看出自己哪些内容需要学习，特别是 Using Ethereum 部分，链接：https://ethereum.org/en/quizzes/，今天就先整理一下错题。

1. 以太坊的共识层在被叫做共识层之前是被称作 Eth2 的，但是在升级之后如果有人拿 Eth2 来表示 Eth 的升级后版本，一般都是诈骗的。
2. 这道题问的是哪个选项是用来扩展以太坊的，Layer2 rollups 捆绑交易、Proto-Danksharding 为这些数据创建廉价的临时存储、Danksharding 共享存储负担，使所有 validator 都能访问。
3. Proto-Danksharding 是如何在 rollups 上降低 rollup transaction 花费的？
   Proto-Danksharding 创建了临时的 ”blob“ 存储，可以让 rollups 更加便宜的将结果发布到主网。

4. 对于 rollups 扩展以太坊的关键的下一步是什么？
   将 sequencers 和 provers 的责任分散到更多人的身上。对 rollup 的控制是从中心化开始的，因为这有助于启动，但也使 network 容易收到审查。将交易过程去中心化处理，使任何人都可以参与其中，对防止网络被妥协的可能性至关重要。

5. 如果你的节点离线了会发生什么？
   当你的节点不在线的时候，它将不能从节点同步接收到新的交易和区块，所以会跟链上的当前状态脱节。重新连接到网络将使你的节点软件重新同步，以便再次完全正常运行。

6. 一个 validator 要获得盈利的正常运行时间是多少？
   答案是大约 50%，处于离线状态的时间如果是 50%，仍然可以实现盈利，但是比那些更加可靠可用的 validator 要少。

7. 如果一个 validator 离线了会发生什么？
   当 validator 不可用时，会产生小额的 inactivity 惩罚，大约等于正确证明所获奖励的 75%。在罕见/极端情况下，如果 network 未完成最终状态（即超过 network 的 1/3 也处于离线状态），这些惩罚将显著增加。当 validator 恢复在线时，奖励会恢复，不会发生惩罚。

8. 运行被大多数其他 validator 使用的 client 会是你在该 client 存在软件错误的情况下面临被 slashed 的风险，而运行少数人使用的 client 可以防止这种情况发生。
