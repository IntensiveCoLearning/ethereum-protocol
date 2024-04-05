# andrea

区块链萌新

## Notes

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
