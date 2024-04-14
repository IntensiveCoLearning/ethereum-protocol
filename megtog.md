# megtog

<https://twitter.com/megtog>

## Notes

### 2024.4.14

周末感觉没啥学习动力

今天看了一点这个视频 [Eth2 Collaboration Culture ](https://youtu.be/ixGeIEgdf4Y?t=530)

发现了这个 

<https://twitter.com/drakefjustin/status/1102911207785287680>

<https://github.com/protolambda/beacon-challenge>


所以 protolambda 是赢了这个 10 eth 的 bounty 之后加入 EF 工作的呀

#### TODO
一些 2019 年那个时期资料，后续梳理历史的时候再看

<https://twitter.com/drakefjustin/status/1102916908326797312>

<https://youtu.be/83DGZPJoyPQ>


### 2024.4.12

今天又又没搞 spec 学习 :-(

看到 unchained 的 learn 板块发了一篇文章 [What is EIP-7251 (MaxEB) in Ethereum? A Beginner’s Guide](https://unchainedcrypto.com/eip-7251-maxeb-ethereum/)。 就看这篇文章当今天的学习吧（话说 learn 板块的文章好像都没有作者署名，不知道怎么考虑的）

> Validators won’t have to fully exit and re-enter the protocol to merge their effective balances.

正好不太明白 MaxEB 里 [Consolidation](https://hackmd.io/@philknows/S1JbLXmlA#Consolidations) 的意指，就是这个合并的意思么

看到这个 hackmd 里说这个 eip 还会修改，方便 lido 兼容，很多细节需要学习

### 2024.4.11

今天又没搞 spec 学习

找了 Casper FFG 的文章来看，文章分 4 个章节，今天也才看了 2 页（相当于第二节看了个开头）。 过去也尝试看过，但看不进去。 趁着这次学习，感觉能看懂多一些了

和现在比较一致的用词：
  * source
  * target
  * justified

变化大的用词是，checkpoint 变成了 epoch

可以看出当时想往 eip1011 演进，用 PoW 来出块

比较感慨的是，一个作者现在在监狱里

### 2024.4.10

今天又是比较懒散

* 看了一半 [Week 8D Consensus Client 架构视频](https://www.youtube.com/live/cZ33bfGXzOc?si=HyqNP4ubVKTtBL3m)
  * Eip 7251(MaxEB) 里有修改 [initial penalty](https://eth2book.info/capella/part2/incentives/slashing/#the-initial-penalty)
* spec 里 `process_slashings` 这个函数再仔细看看
  * `adjusted_total_slashing_balance = min(sum(state.slashings) * PROPORTIONAL_SLASHING_MULTIPLIER, total_balance)` 的计算

### 2024.4.8

今天搁置了 spec 学习

看了一篇 [ICO 旧文](https://web.archive.org/web/20210818204534/https://www.newyorker.com/tech/annals-of-technology/pumpers-dumpers-and-shills-the-skycoin-saga)

> He boasted that Obelisk was written by Houwu Chen, a developer who had worked on Ethereum, the second-most popular cryptocurrency platform. He posted an academic paper written by Chen on the Skycoin Web site, claiming that it was a Skycoin white paper. But when Stephens reached out to Chen, he got no response. “We kept trying to chase people down to ask details, and it was slowly revealed it just . . . didn’t exist,” he told me. In a later discussion about technical details, a Skycoin “community manager” told coinholders that “it’s too advanced to share” and that the company “can’t trust the public with it.” When pressed, Smietana wrote, of Chen, “He is a recluse, I doubt anyone can contact him or he would respond.” Smietana eventually told me that Chen had taken a payment of a million Skycoins for his work on the white paper and then left the project. (Chen declined to comment, saying, “Just don’t put my name in the article. That’s my statement.”)

  * 搜了搜关于 Houwu Chen 的信息，大概是早期 [Pyethereum 的开发者](https://blog.ethereum.org/2014/04/10/pyethereum-and-serpent-programming-guide)
    *  [照片](https://web.archive.org/web/20140509173418/https://www.ethereum.org/)
    *  [Sky: Opinion Dynamics Based Consensus for P2P Network with Trust Relationships](https://arxiv.org/abs/1501.06238v8) 这文章粗看应该挺有料的，但后来撤稿了。这文章致谢了万向的[BlockGrant X](https://web.archive.org/web/20151219053234/http://blockchainlabs.org/blockgrant-x-en/)，但看不出来具体是哪项。


### 2024.4.7

读了一些旧历史

* [Deprecating EIP 1011 in favor of a Casper+Sharding design](https://medium.com/@djrtwo/casper-%EF%B8%8F-sharding-28a90077f121) 发于 Jun 16 2018
  * 之前的想法 <https://twitter.com/megtog/status/1539972179961319427>
  * [eip-1011](https://eips.ethereum.org/EIPS/eip-1011) 结合了 PoW + PoS
  * consensus-spec 项目是在这之后启动的（Sep 20 2018）
 
* 在 consensus-spec 的 git 记录里考古
  * vitalik <https://github.com/ethereum/research/commit/093aad4a7820fd4a06be370a5b1b1586d39b48c3>
  * protolambda <https://github.com/ethereum/consensus-specs/issues/793>


### 2024.4.6

这几天清明假期有点疏于学习

这几天计划结合 MaxEB 学习 CL Specs
* 结合 week 6 内容，学习 pyspec 使用
* 学习 python 的依赖包相关知识 <https://peps.python.org/pep-0508/#extras>
* 后续学习使用 <https://github.com/ethereum/consensus-spec-tests>

### 2024.4.4 MaxEB

> Understandably, there is a lot of hype around MaxEB due to its capability to cap/reduce validator sizes, decrease network messages, and save bandwidth for solo stakers. However, people tend to overlook EIP-7549: Move committee index outside Attestation
>
> Which achieves similar benefits without necessitating validator consolidation. Assuming perfect aggregation and a 1M validator 64-committee size, we can observe a reduction from 18KB to 4KB
>
> Both solutions will coexist and complement each other, but it's essential not to underestimate the advantages of having a superior attestation aggregation structure. See @mkalinin2's notes for more detail:
>
> https://hackmd.io/@n0ble/eip7549_onchain_aggregates

<https://twitter.com/terencechain/status/1771231145583137043>

<https://ethresear.ch/t/increase-the-max-effective-balance-a-modest-proposal/15801>

