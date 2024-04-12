# Chloe

Hi I'm Chloe, currently PM of EIP Fun. I've followed the EPF study group since Week 0 and super excited to join the intensive study group to level up my learning and study & discuss together.

- Twitter/X: https://twitter.com/Chloe_zhuX
- Telegram: https://t.me/chloe_zhu


## Notes
### 2024.4.3

My previous notes on EPF study group: https://twitter.com/EIPFun/status/1775443183884763521, incl.
- Week 1 Ethereum protocol 101
- Week 2 Execution layer overview
- Week 3 Consensus layer overview
- Week 4 Testing & Security overview
- Week 5 Ethereum roadmap overview
- Week 6D CL & EL specs
- Week 6R Sharindg & DAS

### 2024.4.5 Verkle tree study
Youtube video:
- Guillaume Ballet - The Verge: Converting the Ethereum state to Verkle trees
- https://www.youtube.com/watch?v=F1Ne19Vew6w

Brief notes
- The Verge
    - The goal
        - Make the blocks self-contained execution units → New node operators don’t need any prerequisite eg. sync action
    - Major changes needed
        - Change the state trie format from a Merkle Patricia Tree (MPT) to a Verkle Tree (VKT)
        - Adding state diffs to the block
        - In the current MPT, all the values are accessible through a key and the key is a hash. When changing to VKT, key rehashing process is needed.
- Converting the state tree to Verkle requires a lot of work
    - In 1 year (from July 2023), there will be c.1bn leaves in the tree, which is a lot of data take days/ months to convert
    - It’s the riskiest change so far, no mistake is allowed
    - There are simply too much data for an instant conversion
- The overlay method: the favoured conversion method
    - How does it work?
        - Freeze the current MPT at a fork block
        - Lay another tree, i.e. the VKT, on top
        - Every value then goes to the new tree
        - In every block, n values were taken from the MPT and their keys were converted then inserted in the VKT
        - The conversion ends when all leaves from the MPT transferred to the VKT
        - Internal MPT nodes will be deleted when the block is finalized (but can be stored else where)
    - Performance
        - Full sync under the process will probably takes 2 weeks to 1 month
        - The process will not affect the block production
    - Timeline
        - If N = 10k, the conversion will take 15 days; if N = 5k, 1 month; if N = 1k, 6 months
        - Why not N = 100k
            - Consequence: more leaves get translated per block → more nodes dropping from the network
            - Tradeoff: # of machines that can follow <> keep the conversion short enough
    - Current result
        - An AMD64 machine from 2017 with 16GB of RAM can handle 10k leaves
        - Rock 5B & cheap ARM machine may handle c.5k leaves
    - Trade offs
        - Pros
            - Conversion in consensus
                - Easy to reorg, sync, debug, and suspend & resume
            - Code for the transition is reused in sync code
            - No need for extra disk space
        - Cons
            - Lower power machine (eg. rock 5b) can drop from the network
            - Long process that can potentially interfere with emergency releases
            - Gas becomes more expensive during the transition (to find some data may need read both MPT and VKT)
    - Backup methods (To do)
        - Offline conversion
        - Bulk conversion
        - State expiry

### 2024.4.6 Week 7R Verkle tree summary notes
- Review & Summarize Week 7 Verkle tree lecture: https://ab9jvcjkej.feishu.cn/docx/C6VMdpDDHoRq2XxGmKicy0E3nng

### 2024.4.7 Working on Week 7D Reth (EL client) Architecture notes
Intro on Reth
Recap on Reth
- What is Reth?
  - Reth is an Ethereum execution client, which started in Sep 2022
    - Introducing Reth blog: https://www.paradigm.xyz/2022/12/reth
  - Alpha stage released in June 2023
    - Releasing Reth blog: https://www.paradigm.xyz/2023/06/reth-alpha
  - Beta stage released in March 2024
    - Announcing Reth beta blog: https://www.paradigm.xyz/2024/03/reth-beta
- Why Reth?
  - Client diversity: Need more client implementations for staking
  - Talent resilience: Need more clients to onboard more new core devs, and more eyes on Ethereum protocols
  - Client scalability: Need clients built for a high gas per second L2 world
  - Code extensibility: Need clients that are easy to extend with principle, without cowboy forking/ rebasing
- 2024 Goals
  - Credible alternative client for L1 Ethereum usage
  - Fastest L2 EVM client
  - State of the art framework for building EVM infra
- What has Reth done so far?
  - Reth github: https://github.com/paradigmxyz/reth
  - Inclusive open source culture & shipping
- Reth's performance benchmark
  - Sync speed
    - Historical sync: 2-4k mg/second
      - Sync historical blocks
    - Live sync: 100-200 mg/second
      - incl. new blocks and check state root for every block
  - Tools used for benchmarking
    - Flood: a load testing tool for benchmarking EVM nodes over RPC
      - Github: https://github.com/paradigmxyz/flood
    - Cryo: the easiest way to extract blockchain data to parquet, csv, json, or a python dataframe
      - Github: https://github.com/paradigmxyz/cryo
- Beta version: 0.2.0 beta.4
  - Current status
    - L1: Cancun ready, tested on Devnets/ Sepolia etc.
    - L2: Support OP stack for usage in L2/ L3s, aka OP Reth
    - Stress tested on EF's EVM fuzzing/ chaos testing, for staking & RPC
    - State of the art performance on all axes except block notifications
    - Snapshot sync done by the Merkle: https://snapshots.merkle.io/
    - Full JSON-RPC incl. Geth-style & Parity style traces
    - Protocol guild/ RetroPGF recipients for impact, to be redistributed
  - Docs
    - Extensive Docs: https://paradigmxyz.github.io/reth/
- Production ready?
  - Production ready in May
    - Reth audit with Sigma Prime (Lighthouse)
    - REVM fuzzing engagement with Guido Vranken (#1 ETH bug bounty leaderboard)
2024 Roadmap
- 3 tracks & mission
  - Core development: Ethereum resilience
    - Cancun: already shipped
    - Ship Electra ASAP: EOF, Verkle tries, Account abstraction
    - Contribute to core dev process with precise writing & benchmarking
  - Performance: Maximize gas per second
    - Parallel EVM: Parallelized execution for any historical/ live sync, builder, or sequencer use cases. Each one is different.
    - JIT/AOT EVM: Run native code instead of interpreter overhead
    - Optimized state commitment: Parallelized state root calculation + new algos
    - Optimal database: Replace MDBX with firewood/ MonadDB-style DB or new perfect hash table index-based design
    - Rigorous benchmarking: Regression testing in CL, aggressive systems optimization
  - Reth Kernel: State of the art EVM infra in <1 day
    - Reth Kernel is the SDK for building bleeding-edge EVM infra
    - Node builder API: Puggable componenets, stop forking nodes
    - Library usage: Build p2p crawlers, indexers, simulations APIs, MEV bots etc.
    - Import Rust node infra and run as one:
      - Rethhouse = Reth + Lighthouse = CL + EL in 1 binary
      - Reth + Helios = Light client CL + EL in 1 binary
      - OP Reth + Reth + Helios + Magi = L1 CL/EL + L2 CL/EL in 1 binary
    - Project idea: Testnet rollup w/ customer EIPs?

### 2024.4.8 Working on Week 8D Teku (CL client) Architecture brief notes
Guest speaker
- Paul Harris, Lead protocol engineer at Teku
    - Senior staff blockchain protocol engineer at Consensys
    - CL client Teku developer
    - Beacon-api maintainer
    - Keymanager-api maintainer

Summary notes
Teku CL client
- Team of Teku
  - 7 software engineers: 4 in EU, 3 in APAC
- High level overview
  - Key drivers of each CL client
    - CL spec
    - Execution API: interface to CL
    - Standard REST api's: beacon-api, keymanager-api, builder-api
    - Client team direction: eg. Teku currently doesn't implement lightclient
- Rules: CL and EL both define open specs
  - PoS is bascially a distributed game
    - Interactions are fairly well defined, and there are rules & grey areas
  - CL uses executable specs
    - Tests are written for the specs, and can be run on all implementations
- Key features of Teku: Aim to be the client of choice for institutional stakers
  - Written in Java
  - Fairly extensive metrics (prometheus/ grafana)
  - Focuses on testing (incl. unit, integration, system tests)
  - Relatively clear logging
  - Maintain 1 code-stream generally (aka master development), and release frequently
  - Well documented, open source, and strive to offer good support
  - Teku is node operator, and also runs the Teku client

### 2024.4.10 Notes on SNARK proving ASICs
Guest speaker: Justin Drake @zksummit11

- Instant proving can incur
    - instant proving (proof latency < slot time) → synchronous composability
    - light validators
    - EVM-in-EVM precompile → native rollups
- For builder there is also incentives
    - synchronous composability → more txns
    - light validators → faster blocks
    - native rollups → bigger blocks
    - overall more MEV
- Companies that work on zkASICs
    - dedicated companies: ASSEAL, Cysic, Fabric
    - relevant companies: Bitmain, Semisand, SuperScalar, ZKTo
    - others: Auradine, Ingonyama, SupraNational

### 2024.4.12 Notes on EIP 3074 <> 4337
Source: https://hackmd.io/@matt/note-on-3074

How 3074 different from 4337
- 3074’s Main goal is to delegate control of EOA to a smart contract. Stolen key means total loss. There is no consideration for tx sponsoring/ relaying.
- 3074 allows EOAs to be used within 4337, and set the stage for future EIPs, which could allow EOAs to permanently upgrade to smart contract wallets.

Beef between 3074 and 4337
- Previously, there are some people from the 4337 camp complained about that 3074 would fragment the community, where devs would build momentum on separate EIPs.
- 3074 could support the multi EVM chain future more easily than 4337.

Weakness of 3074
- Inability to spend ETH from the authorized account
