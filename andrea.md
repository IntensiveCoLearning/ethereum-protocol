# andrea

区块链萌新

## Notes
### 2024.4.7
DFS我昨天看的文章：[Inevitable Ethereum - World Computer](https://inevitableeth.com/home/ethereum/world-computer)
- counter-party risk
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
