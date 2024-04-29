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

### 2024.4.13 Notes on Evolving Ethereum With Improvement Proposals | Pooja Ranjan
- 观看 EthDenver EIP 相关视频，并整理笔记 Evolving Ethereum With Improvement Proposals | Pooja Ranjan - EthCatHerders
- 笔记：https://forum.plancker.org/t/evolving-ethereum-with-improvement-proposals-pooja-ranjan-ethcatherders/379

### 2024.4.14 Week 8R protocol services notes
- Link: https://twitter.com/EIPFun/status/1779616224940318890

### 2024.4.15 Week 9D Testing & Prototyping
- Slides link: https://docs.google.com/presentation/d/1c95eV_55ZlojL9yE1OOPHUPjLyAU8xdh/edit?rtpof=true&sd=true
- What was complicated about the testing?
  - Over 20 client comlinations need to be tested & regressions can sneak in easily
  - Debugging can also be difficult
  - Competences for CLs and ELs are separate
- What are devnets?
  - Devnets are short lived eg. from 30min to a few months, compared to testnets
  - A testing mirror of the Ethereum base layer
  - Contain EL/CL/validators, setup in a config that we want to test
  - Pectra devnet will be setup soon
  - Verkle devnet is already on
- What does testing look like today?
  - Pectra: current upcoming fork
  - Verkle: upcoming future fork
  - Features that are proposed for future forks, prototyping
    - IL
    - EIP 7441 (whisk)
  - Client optimisations
    - EthereumJS snap sync testnet
    - Bigboi beaconchain tests for blob/validator limits
- Local testing: 
  - Hard to test changes quickly, as it normally needs a lot of coordination
  - Solution: local testing
    - Move to local devnets enables faster iterations: enter Kurtosis
    - Able to work async on features
    - Configurable locally: 3s slot times, quick forks, MEV workflow
    - Scalable: to whatever extent kurbernets/ docker allows
  - Github repo: https://github.com/kurtosis-tech/ethereum-package
  - Install instruction: https://docs.kurtosis.com/install/
  - Example of configured with YAML
  - Example of MEV local testing
- How do I prototype?
  - Kurtosis works on the concept of "allow everything to be overridden"
  - To test protocol changes, we can override the client images
  - To test new tools, run an existing network and connect your tool to it
  - To test quick forks, we can override the network parameters
- Example of prototype testing: Verkle transition
- What comes after local testing?
  - Remote/ public devnets
    - Devnets used to be error prone and time consuming
    - Easy drift btw setup configs of various testnets due to customizations
  - Solution 
    - Move barebones logics upstream into role
    - Move generic components into its own tool, eg. Genesis
    - Make tooling independent of repo/ testnet (with GitOps)
    - Generalize setups for all testnets
- Shadowforks
  - Allow us to check compatibility across all clients through the entire lifecycle
  - Shadow forks allow us to stress test the clients with real state and tx load
  - Act as release test which triggers real world edge cases, before we recommend the releases to the general public
- Handy tools overview
  - Overview 
    - https://ethpandaops.io/
    - notes.ethereum.org/@parithosh/testing-overview-doc
    - Tools are all open-sourced (MIT). Some configs depending on the projects sometimes won't be open-sourced.
  - Template-devnets
    - Contain everything to config for any type of testnet
    - Use Terraform to spin up cloud instances and Ansible to deploy the network
    - Useful if you want to run nodes on a larger scale and local testing tools are inadequate
  - Assertoor
    - Tool to assert network level expectations, eg. Can a network handle deposits, handle every opcode being called, handle a reorg etc.
    - Similar to Hive: Hive <> Single node, Assertoor <> Network side
    - Can be run locally via kurtosis or integrated into a CL
  - Forky
    - Ethereum forkchoice visualizer
    - Can display the forkchoice of a live node
    - Help debug forkchoice related issues
  - Tracoor
    - Ethereum trace explorer
    - Can display a collection of traces & states of EL & CL blocks/ slots
    - Help debug network related issues
    - Recent blog post [][]
  - Dora
    - Lightweight slot explorer for the Ethereum beacon chain
    - Extendable, low level access to DB 
    - Links 
      - https://github.com/ethpandaops/dora
      - https://beaconlight.ephemery.dev/
  - Xatu
    - Xatu data is fed into an analysis pipeline to get data we care about
    - The visualization is handled by Grafana, but the DB can directly be queried as well

### 2024.4.16 Week 9R History expiry pre-reading
Statelessness, state expiry and history expiry: https://ethereum.org/en/roadmap/statelessness/

**Goal**
- Let modest hardware (eg. Mobile phones, micro-computers, home computers etc.) have the ability to run Ethereum nodes to achieve decentralization

**Current problem**
- High disk space requirement is the main barrier, primarily due to the need to store large Ethereum's state data
- Currently a fast 2TB SSD is recommended for running a full node

**Solution: Reduce storage for nodes**
- Data expiry
  - History expiry
    - Enable nodes to discard state data older than X blocks, but does not change how Ethereum client's handle state data
    - Option for clients to request historical data from peers
      - Portal network: p2p network for serving historical data
    - Risk
      - Moving the responsibility for providing data guarantee outside of the Ethereum core protocol could introduce new censorship risks
  - State expiry
    - Allow state data that is not used frequently to become inactive. Inactive data can be ignored by clients until it is resurrected
    - Options for state expiry
      - Expire by rent: charging "rent" to accounts and expiring them when their rent reaches 0
      - Expire by time: making accounts inactive if there is no reading/ writing for some time
- Statelessness 
  - Weak statelessness
    - Only block producers need access to full state data
    - Other nodes can verify blocks without a local state database
    - Prerequest: Verkle tree & PBS
  - Strong statelessness
    - No nodes need access to the full state data

**Current progress**
- Prerequest: Verkle tree and PBS
- Research in progress: weak statelessness, history expiry, and state expire. If state expiry is implemented first, then there may be no need to implement history expiry

### 2024.4.18 Portal network study

The Portal Network by Piper Merriam - Lightweight protocol for everyone
https://www.youtube.com/watch?v=0stc9jnQLXA
- What's the portal network
  - 5 new decentralized storage networks
    - Beacon light client: beacon chain light protocol data
    - State network: account & contract storage
    - Transaction gossip: lightweight mempool
    - History network: headers, block bodies, receipts
    - Canonical txn index: TxHash > Hash, Index
- Design goals
  - Users-focused
  - Lightweight
  - Elimination of syncing
  - Scalable in terms of # of participants
- Project status as of Devcon Bogota
  - In full development stage
  - 3 portal client implementations: Trin, Ultralight, Fluffy
  - Rough timeline: fully operational history network -> beacon light network -> state & transaction gossip networks

### 2024.4.19 Notes on Vitalik's blog on state expiry & statelessness roadmap
https://notes.ethereum.org/@vbuterin/verkle_and_state_expiry_proposal

- The problem
  - State storage is increasing at a fast level, which puts pressure on hardware requirements and disk space 
- Potential solution
  - State expiry: remove state that hasn't been recently accessed from the state
  - Weak statelessness: only require block proposer to store state, and allow other nodes to verify blocks statelessly (pre-request is a switch to Verkle trees)
- History ideas & works on state expiry & statelessness
  - The Stateless Client Concept, original ethresear.ch post (2017): https://ethresear.ch/t/the-stateless-client-concept/172 (see also EthHub)
  - State rent (precursor to state expiry), original 2015 proposal: https://github.com/ethereum/EIPs/issues/35
  - ReGenesis (Alexey Akhunov’s proposal, can be described as a form of state expiry + history expiry): https://medium.com/@mandrigin/regenesis-explained-97540f457807
  - Verkle trees: https://notes.ethereum.org/_N1mutVERDKtqGIEYc-Flw
  - Presentation on bounding witness sizes (Youtube): https://www.youtube.com/watch?v=qQpvkxKso2E
  - A theory of state size management (Feb 2021): https://hackmd.io/@vbuterin/state_size_management
  - Resurrection-conflict-minimized state bounding: https://ethresear.ch/t/resurrection-conflict-minimized-state-bounding-take-2/8739
  - A few paths to statelessness and state expiry: https://hackmd.io/@vbuterin/state_expiry_paths
- How does state expiry work
  - 2 principles
    - Only the most recent tree can be modified
    - Full nodes (incl. Block proposers) are expected to only hold the most recent 2 trees, so only objects in the most recent 2 trees can be read without a witness
  - Hybrid state regime
    - Consensus nodes need to store state that was accessed or modified recently
    - But can use the witness-base stateless client approach to verify older state
- Roadmap
  - Hardfork 1: Transform MPT to VKT
  - Address period expansion: addresses are extended from 20 to 32 bytes, and the new address format incl. "address periods" to allow new contracts fill new storage slots without the need to provide witness
  - Hardfork 2: State expiry

### 2024.4.21 Week 9R History expiry notes
- Notes: https://twitter.com/EIPFun/status/1782120924172468636

### 2024.4.22 Week 10D Precompiles notes
- 3 kinds of precompiles
  - Precompiles: tasks you could do with the EVM, but are too expensive/ slow
  - System contracts: tasks & side effects you cannot do with the EVM
  - Predeployed contracts: contracts that are part of the initial state
- Ethereum mainnet precompiles: https://www.evm.codes/precompiled
  - 0x01 - ecRecover
  - 0x02 - SHA2-256
  - 0x03 - RIPEMD-160
  - 0x04 - identity
  - 0x05 - modexp (Byzantium)
  - 0x06, 0x07, 0x08 - ecAdd, ecMul, and ecPairing on alt_bn128 (Byzantium)
  - 0x09 - Blake2B F Function (Istanbul)
  - 0x0A - KZG point evaluation (Cancun)
- EVM view
- Design issues in precompiles
  - All boundary conditions must be specified
  - Gas should scale with effort
    - Execution: algo can hide problems
    - Input: variable input should always be charged
  - Costs should account for worst case
- Implementation strategies
  - Implement with client software
  - x
- How it's implemented
  - Besu
  - Geth
  - Nethermind
  - Reth
- Implementation strategies
  - Implement with client software
  - Implement 
System contracts
- System contract use cases
  - Access L1/L2 bridging
    - Arbitrum, Optimism, zk chains
  - Access foreign host chain services
    - Moonbeam, Aurora, Hedera
  - Advanced services (coming soon)
    - Fhenix (FHE), Ritual (AI model execution)
- Typical L2 system contract uses: https://www.rollup.codes/
  - L1/2 communications
  - treasury/ fee vault mgmt
  - security/ admin tasks
  - Chain info queries
- Notable design choices in L2 contracts
  - Use of solidity ABI for precompiled access
  - Mixed API designs
  - Mixed permance
  - Mixed implementation strategies
  - Mixed contract address deployments
- Foreign host chain services
  - altL1 token access
  - altL2 account tools
  - Zk features
- Security & system contracts
  - Precompiles don't share Ethereum's memory model
  - DELEGATECALLS can impersonate SENDERs via callback
  - Best to ban Delegatecalls into precomiples
  - Ensure all actions are revertable
- Precompile futures
  - There is resistance to adding new mainnet precompiles
    - BLS-9 separate functions
  - Rollcall is standdardinzing L2 precompiles
    - ECDSA (secp256r1) verification
  - EVMMAX (modular math extensions) may reduce the demand
    - Aspirationally to be w/in 2x gas cost
- Progressive precompile
  - New quasi-proposal to "cannonize" well known contracts
  - How to handle gas is unresolved
  - Needs better math support (eg. EVMMAX)
  - Mixed execution example: EIP 4788 - canonical EVM code exists
    - Execute the contract - Geth/ Reth
    - Native evaluation - Besu/ Nethermind

### 2024.4.24 Notes on Fork choice advanced research
Week 10 最后一节课，听得我真是头大，需要重新回顾视频好几遍
Deck link：https://docs.google.com/presentation/d/1Hrk-0x7N18qHwy9d7DeONdOVpA_6GPOqG7xxf6TtaGw/edit#slide=id.g1f7e27a7462_3_890

- Gasper recap
  - GHOST (Greediest Heaviest Observed SubTree)
  - LMD-GHOST (Latest Message Driven GHOST)
    - Aka Vote driven GHOST
    - LMD-GHOST as a protocol
  - Casper FFG
  - Ethereum today
  - Modelled as an Ebb-and-flow protocol
    - Aim
      - Dynamic availability
      - Finality
      - Prefix 
  - Hybrid fork-choie
- Problems of LMD-GHOST
  - Simple ex-ante reorg
    - Solution: Proposer boost
      - New block proposals have a temporary weight boost during their slot
      - In practice, set to 40%
  - Balancing attacks
    - Solution: Proposer boost still works in this case
- Designing a theoretically secure available chain
  - Improving on proposer boost: View-merge
  - RLMD-GHOST
- SSF protocol
- Fork choice in the Ethereum Roadmap
  - SSF or fast finality
  - ePBS
  - DAS
- Q&A
  - If possible, can you elaborate on what's the BLS signature count limit / slot in SSF scenario?

### 2024.4.25-28 Notes on PoS evolution
Source: https://github.com/ethereum/pos-evolution/blob/master/pos-evolution.md

- System model
  - Validators 
    - Validators are assigned a protocol to follow
    - A protocol for V consists of a collection of programs with instructions for all validators
    - Each validator has a deposit/ or stake
  - Failures
    - A validator that follows its protocol during an execution is called honest
    - A faulty validator may crash or deviate arbitrarily from its spec -> Byzantine faults
    - Assume the existence of a probabilistic poly-time adversary A, that can choose up to F validators to corrupt
  - Links
    - Assume a best-effort gossip primitive that will reach all validators is available
  - Time & sleepiness
    - Time is divided into discrete rounds & validators have synchronized clocks
    - In a synchronous network, the message delay is upper-bounded by a constant Δ rounds, with Δ known to the protocol.
    - In a partially synchronous network in the sleepy model, communication is asynchronous until a global stabilization time (GST), after which communication becomes synchronous
      - Honest validators sleep and wake up until a global awake time (GAT), after which all validators are awake. 
      - Adversary validators are always awake.
  - View
    - A view is a subset of all the messages that a validator has received. 
    - The notion of view is local for validators.
- Gasper: a PoS consensus protocol by combining FFG Casper, a partially synchronous consensus protocol, and a synchronous consensus protocol named LMD-GHOST.
  - FFG (Friendly finality gadget) Casper
    - Overivew
      - Goal: finalize the proposed blocks, ensuring their safety even during potential network partitions
      - Feature
        - Accountability, i.e. If a validator violates some rule, it's possible to detect the violation and know which one violated -> allow the system to penalize/ slash the Byzantine validator
      - Casper Mechanism
        - 2-phase traditional propose-vote-based BFT mechanism
        - Designed as a gadget that works on top of a provided blockchain protocol
        - There is no leader in charge of assembling proposals -> generated across honest nodes by an underlying proposal mechanism
    - Checkpoint
      - Process of Casper
        - Validators participate in the protocol by casting votes on blocks in the block-tree -> Messages exchange among validators are votes for blocks
        - Vote message incl.: 2 blocks source & target, and their heights
        - Once a vote has been cast by 2/3 of validators and the checkpoint of the source block is justified, and the target block becomes justified
      - Casper has 2 properties
        - Accountable safety: 2 conflicting checkpoints imply that more than 1/3 adversarial stake can be detected
        - Plausible liveness: Possible to produce new finalized checkpoints
    - Voluntary exit
      - Dynasty of a block: the number of finalized checkpoints in the chain from root to the parent of block b
      - Start dynasty: When a would-be validator’s deposit message is included in a block with dynasty 𝑑, then the validator 𝑣𝑖 will join the validator set at first block with dynasty 𝑑+2
      - End dynasty:  If validator 𝑣𝑖’s withdraw message is included in a block with dynasty 𝑑, it similarly leaves the validator set at the first block with dynasty 𝑑+2

    == TODO ==
  - LMD-GHOST
    - Latest message
  - FFG Casper + (H)LMD-GHOST = Gasper
    - Beacon state
    - Committee
    - Proposer
    - Beacon block
    - Attestation
    - Justification & finalization
    - Fork choice
    - Slashing
    - More on slashing
- Properties of Gasper
  - Availability-finality dilemma
- Extra: Weak subjectivity
- Problem & solution
  - Problem: balancing attack
    - Part of the solution: Proposer weight boosting
    - Other part of the solution: Equivocation discounting
  - Problem: Avalanche attack (solved with equivocation discounting)
  - Problem: Ex-Ante reorg
    - Solution: View-merge as a replacement for proposer weight boosting




