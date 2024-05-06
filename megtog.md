# megtog

<https://twitter.com/megtog>

## Notes

### 2024.5.06

#### dumpState

逛 protolambda 的 github 看到

<https://github.com/ethereum-optimism/optimism/pull/10387>

<https://github.com/ethereum-optimism/optimism/blob/0f11f2aacb22bafa926c04dcda285dcbc101f97f/packages/contracts-bedrock/scripts/Deploy.s.sol#L269-L273>
```
    function runWithStateDump() public {
        vm.chainId(cfg.l1ChainID());
        _run();
        vm.dumpState(Config.stateDumpPath(""));
    }
```

vm 应该是来自继承

```
contract Deploy is Deployer {
  ...
}

abstract contract Deployer is Script, Artifacts {
  ...
}
```

<https://book.getfoundry.sh/tutorials/solidity-scripting#writing-the-script>

下一步计划用下 [anvil_dumpState](https://book.getfoundry.sh/reference/anvil/#custom-methods)

hardhat
<https://github.com/NomicFoundation/hardhat/issues/3599>


### 2024.5.05

#### BLS 以及一些链接

每次看密码学相关的内容，都会感到自己多么无知

<https://ethresear.ch/t/pragmatic-signature-aggregation-with-bls/2105>

> The term “curve” is massively overloaded, its an abbreviated term that describes many different things. Certainly the elliptic curve equation y^2 = x^3 + ax + b is the same, but they changed the a and b parameters. Between bn128 and alt_bn128 the a and b parameters are the exact same; only the G2 generator point is different (if I’m not mistaken). I spent some time figuring all this out and it was more difficult than it should be, not because the math is complex (which it is) but because the codebases tend to have large constants strewn about without any comments/explanation, though they can often be derived from rather simple formulas

<https://ethresear.ch/t/precompiled-snark-pairing-for-bls-signatures/3196/6>

<https://github.com/ethereum/py_ecc/pull/3>

#### Consensys v. SEC 锐评

> You wouldn’t know it because crypto lawyers are mostly class acts when it comes to “praise publicly, scold privately.”

<https://twitter.com/AFDudley0/status/1784355377250791865>

### 2024.5.04

#### 学了新词 Proof of custody

op blog 上看到

<https://blog.oplabs.co/the-collective-triumph-of-4844/>

<https://ethresear.ch/t/a-non-interactive-proof-of-custody-using-polynomial-commitments/5692>

但在 [Vitalik 的 proto danksharding faq](https://notes.ethereum.org/@vbuterin/proto_danksharding_faq#What-parts-of-full-danksharding-does-proto-danksharding-implement-and-what-remains-to-be-implemented) 却归类在 What parts of full danksharding does proto-danksharding implement, and what remains to be implemented 的后一类中

不太懂

#### 细节待学习 aggregation_bits

<https://ethereum.stackexchange.com/questions/125970/what-is-the-aggregation-bits-in-beacon-chains-attestation>


### 2024.5.02

假期出去玩了，基本没学习

看了看 <https://nimbus.guide/validator-client-options.html#multiple-beacon-nodes>

> Sentry nodes setups allow separating block production traffic from attestations and sync committee messages, making sure that a separate public IP address is used when proposing blocks. In this setup, there are two beacon nodes

nimbus 的 beacon node 可以运行多个？ 分配不同的角色

感觉很厉害，想再看看，特别是 `aggregated` 是什么，完全没概念


### 2024.5.01

#### 翻推特

看到 Hudson 进了 ETHPanda 的 tg 群

> how consensus is formed around the block number/timing of a hard fork (network upgrade) in Ethereum.

<https://twitter.com/hudsonjameson/status/1661905231767019520>

> Ethereum presale prices

<https://twitter.com/hudsonjameson/status/1657260574797955073>

> the weeks wasted on arguing the finer details for PoS (CBC & FFG's dissection, definition/importance of weak subjectivity) and what defines an L2/side chain/bridged chains (beyond the colloquial definitions).

<https://twitter.com/hudsonjameson/status/1662346559541981191>

> An often overlooked reason to not have more **precompiles** is the increased chance of **chain splits** from clients having to use different implementations of the precompile code. If one client's precompile library has a bug, it will diverge from the others when a tx triggers the bug.

<https://twitter.com/hudsonjameson/status/1642116003906850816>

#### 准备运行节点

<https://nimbus.guide/el-light-client.html>

#### 关注 op 网络升级

<https://docs.optimism.io/builders/node-operators/network-upgrades>

fjord
- <https://github.com/ethereum-optimism/specs/issues/61>
- <https://github.com/ethereum-optimism/specs/issues/73>

### 2024.4.30

> relay: party specialized in DoS protection and networking who validates and routes execution payloads to proposers (trusted by builders for fair routing and by proposers for block validity, accuracy, and data availability)

**MEV-Boost: Merge ready Flashbots Architecture**

<https://ethresear.ch/t/mev-boost-merge-ready-flashbots-architecture/11177>

**Flashbots POS: Merge Architecture**

<https://hackmd.io/@flashbots/HJ3EbzVUY>

这两个链接算是同一个文档吧，只是标题不一样？

下面那个链接是姚老师那儿看到的 <https://twitter.com/TimeOfSand/status/1582270557315211265>

当时看到 Hasu 发推 <https://twitter.com/hasufl/status/1582005123794235395>

> It's a failure of the Ethereum ecosystem that one month after the merge, there is still no neutral relay. All the relays that exist are vertically integrated builder relays.

> Many builders don't trust builder relays because there's no way to know if a relay favors its own internal builder, e.g. by colocating the two.

现在有点能明白在说啥了

**Block-auction ePBS** 还是没看明白，明天继续看

- builder protocol-bids `PB = { value, builderID, txroot }`


### 2024.4.29

又又是迷你学习量，还过了 12 点了

#### Block-auction ePBS

继续读昨天的 **图解P&S** 文章

- Possibility of unconditional payment
- Possibility of trust-minimised proposer/builder interaction
- Consensus/execution separation

为啥前两点设计目标还加个『Possibility』呢

#### 闲话

今天围观 uncommons 里的一次投票，关于一个专栏的奖励

<https://snapshot.org/#/0xuncommons.eth/proposal/0xccbad2da0deca068eda2e06f1828e9da3d28129562df09bfbc950da26b92d8ac>

大概是先有了一次 snapshot 投票，有反对票，下午开会讨论，决定梳理和细化规则，又创建了新投票 （原投票好像在 snapshot 上找不到了，不太熟悉）

我个人感觉 uncommons 应该就这个事件，书写下文字发出来，因为这样的事件才是能让读者更了解 uncommons 的（也可能更有兴趣读）


### 2024.4.26

今天又是迷你学习量

#### precompiles

需要赶紧去把 precomile 那一周的视频看了，了解是怎么调用的


#### eips.wtf

<https://www.eips.wtf/>

能不能加上看到，哪次升级实现了哪个 eip 的功能

#### More pictures about proposers and builders

<https://mirror.xyz/barnabe.eth/QJ6W0mmyOwjec-2zuH6lZb0iEI2aYFB9gE-LHWIMzjQ>

> The allocation of building rights is performed today with MEV-Boost, an out-of-protocol side-car allowing validators to passively listen to offers from builders for their building rights, and strike a deal with builders. The deal is mediated by **relays**, entities trusted by both sides of the deal.

一句话解释什么是 relay


### 2024.4.25

这几天现实生活有点忙，不太有时间学习

Ethereum Beats: Exploring the Dance of Validators and Builders in Ethereum's Festival of Transactions
<https://discord.com/channels/1205546645496795137/1206623768940908566/1232956951667802185>

至少认识到 MEV-Boost 是初代 PBS

### 2024.4.24

今天比较拖延，基本没学啥

这个 2022 年的视频 [Understanding L2: Ordering and Execution](https://youtu.be/RcOaH9GL-78)，看了前几分钟，什么是 sequencer，就是让用户提交 tx 的服务吧

op 是否可以绕过 sequencer 直接提交 tx 呢 （看到 arb 有 inbox 这个概念）

#### 其他

<https://www.reddit.com/r/ethstaker/comments/1c0qmgn/on_issuance_reduction_and_anticorrelation/>

留言看得出来，solo staker 对收益下降还是蛮反对的

<https://twitter.com/terencechain/status/1782890058615324837>

> I think rushing to ship MEV-boost to avoid delaying the merge was a mistake

### 2024.4.23

`justification_and_finalization` 里 9 个测试

```
123_ok_support
123_poor_support
12_ok_support
12_ok_support_messed_target
12_poor_support
234_ok_support
234_poor_support
23_ok_support
23_poor_support
```

可能现在 zrnt 版本有问题，会去读 `meta.yaml`，应该去读 `pre.yaml / post.yaml` 才对呀

但是测试还是能通过，汗。。。

```
meta.yaml // 不存在
pre.ssz
post.ssz
```


### 2024.4.22

出现 `PAUSE` `CONT` 的原因
<https://pkg.go.dev/testing#T.Parallel>

计划学习新 issuance 提案
<https://ethresear.ch/t/reward-curve-with-tempered-issuance-eip-research-post/19171>


### 2024.4.21

继续和 zrnt 玩耍

#### 运行 v0.8.4 版测试

<https://github.com/ethereum/consensus-spec-tests/tree/v0.8.4>

```
zrnt/tests/spec$ du -sh eth2.0-spec-tests/tests/general/ eth2.0-spec-tests/tests/minimal/
25M	eth2.0-spec-tests/tests/general/
87M	eth2.0-spec-tests/tests/minimal/
```

#### `go.sum` 被更改问题

在 discord 上先提问了

#### 先看这个测试

学习 go 语言测试里 `PAUSE` `CONT` 都是咋回事儿

<details>
<summary>TestJustificationAndFinalization</summary>

```
=== RUN   TestJustificationAndFinalization
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_ok_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_ok_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_poor_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_poor_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support_messed_target
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support_messed_target
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_poor_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_poor_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_ok_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_ok_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_poor_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_poor_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_ok_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_ok_support
=== RUN   TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_poor_support
=== PAUSE TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_poor_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_ok_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_ok_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_ok_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support_messed_target
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_poor_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_poor_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_poor_support
=== CONT  TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_poor_support
--- PASS: TestJustificationAndFinalization (0.00s)
    --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization (0.00s)
        --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests (0.00s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_ok_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_poor_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support_messed_target (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/234_ok_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_poor_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_ok_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/123_ok_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/12_poor_support (0.01s)
            --- PASS: TestJustificationAndFinalization/epoch_processing/justification_and_finalization/pyspec_tests/23_poor_support (0.01s)
PASS
```
</details>

### 2024.4.18

发现 ericsson49 研究过不少 eth consensus 时间相关的问题

[Why clock sync matters in Ethereum 2.0](https://hackmd.io/@ericsson49/BJfLjEX-8)

> An alternative approach would be to re-design beacon chain to avoid its reliance on the clock synchronization property. While it seems quite radical, actually it's a traditional approach to build reliable distributed systems, since logical clocks are much easier to implement, ...

搜了搜推特 ericsson49 大概是 <https://twitter.com/alex_vlasov_eth>

另方面想通过 `zrnt/tests/spec/test_util/transition_util.go` 这个文件学习 go 语言

看到 proto 在 2019-08-01 这天的工作真饱满啊

### 2024.4.17

用之前提到的 protolambda 的 go 代码跑了 consensus-spec

```text
zrnt/tests/spec$ go test -count=1 -mod=mod -tags preset_minimal ./test_runners/...
ok  	github.com/protolambda/zrnt/tests/spec/test_runners/epoch_processing	0.642s
ok  	github.com/protolambda/zrnt/tests/spec/test_runners/operations	4.494s
ok  	github.com/protolambda/zrnt/tests/spec/test_runners/sanity	1.913s
ok  	github.com/protolambda/zrnt/tests/spec/test_runners/ssz_static	8.047s
```
<https://github.com/protolambda/zrnt/tree/c223e8d53a8732b65f2891a3ef6d1399ec573cd9>

用的 v0.6.3 测试数据

还学了点 go 语言相关知识

<https://go.dev/ref/mod>


### 2024.4.16

希望带着问题看 consensus-spec 

* 本来 block 是里存 txs 的， 问 beacon chain 的 block 里存了什么信息
  * 肯定是存了 attestations
  * 更深入的问题 beacon chain block，哪些信息存在 block header，哪些信息存在 block body

* block 里肯定要记录 slot， 记录 slot 编号的方式是哪种
  * 选项 A ： 0 - 31 的数字，一个 epoch 结束后重新从 0 开始
  * 选项 B ： 从 0 开始一直增长
 
* 一个 slot 12 秒，我们知道这 12 秒 分成 4秒 + 4秒 + 4 秒三个阶段，每个阶段要做的事情不同
  * 这个信息在 spec 中如何描述


> With 4 years of hindsight it is now clear to me that the beacon chain is needless complicated. With all the latest and greatest research ideas I believe one could redesign the beacon chain from scratch to be ~10x more powerful and ~2x simpler. We obviously need continuity and can't simply declare tabula rasa, but there is definitely an opportunity for massive simplifications and improvements in the future :)

<https://old.reddit.com/r/ethereum/comments/191kke6/ama_we_are_ef_research_pt_11_10_january_2024/khcfkve/>

### 2024.4.15

还是浅层次学习，比较愿意看看历史和八卦 （已经过了午夜了啊啊）

今天关注 Casey Detrio

> The bridge idea is a few months old now, but has not been immediately recognized as something major (at least, not by me). Tracing the history, we can see why it's been under-appreciated. The story begins, of course, with the June 2018 pivot.
> The pivot away from deploying PoS on the main chain, to the new plan of launching Eth2 (PoS + sharding) as a separate new chain (beacon-chain + shards), left us reeling and disoriented. Thrust into a new universe, we became reoriented around eth2, with an eth2-centric vision.

<https://twitter.com/cdetrio/status/1134949255045681152>

之前提到 eip1011，那个计划里是没有 beacon chain，所以也就不需要 merge


2020 十月 Vitalik 在 A rollup-centric ethereum roadmap 里引用了
<https://ethresear.ch/t/phase-one-and-done-eth2-as-a-data-availability-engine/5269>

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

