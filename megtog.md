# megtog

<https://twitter.com/megtog>

## Notes

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

