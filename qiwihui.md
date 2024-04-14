# qiwihui

Hi, I am qiwihui, a python dev. I am interested in Ethereum protocol. Looking forward to learning from you guys.

- Twitter/X: <https://twitter.com/qiwihuix>
- Telegram: <https://t.me/qiwihui>

## Notes

### 2024.4.4

Join the group.

### 2024.4.5

1. Cryptography

    - Hashing
        - A hash function is a versatile one-way cryptographic algorithm that maps an input of any size to a unique output of a fixed length of bits.
        - What hash function can do:
            - Ensure data integrity,
            - Secure against unauthorized modifications,
            - Protect stored passwords, and
            - Operate at different speeds to suit different purposes.
        - example: Secure Hash Algorithm (SHA), Message Digest (MD)
    - Public key Cryptography
        - asymmetric encryption scheme
        - encryption: It allows anyone to encrypt a message that can only be decrypted by the corresponding private key.
        - signing: A digital signature is created by encrypting a hash of the message using the sender’s private key. The recipient can then verify the authenticity of the message by decrypting the digital signature using the sender’s public key and comparing it to the calculated hash of the received message.

2. Merkle tree in Bitcoin

    - how to form merkle tree
      1. Perform hashing to each transaction
      2. put transaction hashes into pairs and perform hash to the pair
      3. pair the hashes again and hash them again until all the hashes meet at a sing hash
      4. the sing hash is call Metkle root and it is the root of the merkle tree
      5. merkle root will be put into the block header
    - make transactions tamper proof, verification donot need whole transactions data

3. p2p network
    - client-server netowrk, central point of storage
    - blockchain p2p network: no central point and no controlling parties, echo nodes/peers keep a copy of the data
    - far less vulnerable being hacked, expoited or lost

4. BitTorrent
    - BitTorrent is designed for a large group of machines to exchange the same file quickly and reliably.
    - Rather than optimizing a single path, BitTorrent tries to take advantage of all available network bandwidth by dividing the transfers needed to retrieve a single large file between multiple cooperating peers. This allows it to frequently outperform the typical model in which clients download only from a central server.

### 2024.4.6

what is Ethereum?
Ethereum is the World Computer. It made up with 3 parts: Ethereum Virtual Machine (EVM), Ethereum Blockchain, Ethereum Network.

1. Vitalik Buterin reveals Ethereum at Bitcoin Miami 2014
2. Ethereum Improvement Proposals, aka EIPs, describe standards for etherem platform, including core protocol spec, client APIs and contract standards.
3. Design principles, only list some of them:
    - Simplicity, Universality, Modularity, etc.
    - Sandwich/encapsulated complexity
    - Freedom, neutrality
4. Ethereum layers:
    - Consensus layer: fork choice, RANDAO, Blobs
    - Execution layer: EVM, mempool, state
    - Validators <--Beacon APIs--> Consensus layer <--Engine APIs--> Execution layer <--JSON-RPC APIs--> User
5. Implementations - EL
    - geth/go
    - reth/rust, etc.
6. Implementations - CL
    - prysm/go
    - lighthouse/rust, etc.
7. Testing
8. Process: Idea -> Research -> Specs -> Implementation -> Testing -> Adoption/Rejection
9. Coordination: Dev calls, EIPs, Ethresear.ch

### 2024.4.7

1. Ethereum is the work computer consisting of 3 parts:
    - Ethereum Virtual Machine (EVM): a Turing-complete distributed state machine.
    - Ethereum Blockchain: the memory & storage of ethereum.
    - Ethereum Network: a group of people and institutions who decide to run an Ethereum node
        - Consensus client
        - Execution Client
        - PoS: a node operator can escrow (stake) 32 ETH to become a validator, with the right to operate and secure Ethereum and to earn ETH
            - slashing: confiscate some/all of a validator's staked ETH and eject them from the validator set.
            - economic incentive
            - trustless trust: Trustless trust comes from credible neutrality. Credible neutrality comes from decentralization.

2. DeFi
    - Programmable Money: code is law
    - Decentralized Finance.
3. Scalability Trilemma
    - pick at most two: scalability, decentralization and security.
4. Scaling Etherem Execution
    - Move execution off-chain while retaining settlement on-chain
    - Rollup
    - Danksharding
5. Enabling True Decentralization
    - operate your own node or use a Node-as-a-Service provider
    - light client
    - Statelessness
6. Decentralized Trustlessness: EigenLayer
7. [Ethereum in 30 secs](https://hackmd.io/@vbuterin/ethereum_in_30_minutes)
    - Accounts: Externally owned account (EOA) and Contracts
    - Gas: the "unit" of resource consumption within Ethereum
        - fee = (base_fee + priority_fee) * gas_used
        - Base fee: burned, value determined by protocol
        - Priority fee: paid to block proposer
        - A maximum of 30,000,000 gas can be spent in each block
    - Trasaction object
        - Tx type -- byte -- Here: 0x02
        - Chain ID -- int -- Which chain is this tx for?
        - Nonce -- int -- Anti-replay value
        - Max priority fee -- int -- Priority fee per gas, for block proposer
        - Max fee -- int -- Max total fee (base + priority) per gas
        - Gas limit -- int -- Max gas this tx is allowed to consume
        - Destination -- address -- Which address this tx goes to
        - Amount -- int -- How much ETH to send
        - Data -- bytes -- Data passed in call if destination is a contract
        - Access list -- List[Tuple[int, List[int]]] -- Accounts and storage slots to more cheaply pre-access
        - Signature (3 fields) -- (bool, int, int) -- Verifies who sent it
    - Proof of Stake consensus
        - deposit 32 ETH to become a validator
        - Each slot, 1/32 of all validators attest to the block created during that slot
        - validator can get 1) in-protocol rewards and 2) priority fees and MEV from transactions as revenue
        - Validators can withdraw at any time, with a delay
    - Fork choice: LMD-GHOST
    - Casper FFG finalization: If > 2/3 of validators online + honest, then after 2 epochs a block is finalized, and cannot be reverted.
    - Merkle trees in Ethereum: Allow for efficiently verifiable proofs that something happened in a block
    - Layer 2 protocols: to inherit security from Ethereum and add higher scalability on top.
    - Future directions:
        - The Merge: Done. But PoS can be improved with single slot finality
        - The Surge: Improved scalability through rollups, danksharding and ZK-SNARKs
        - The Verge: Replacing Merkle trees with more efficient data structures that let Ethereum nodes be much lighter ("stateless clients")
        - The Purge: Clearing out old data and technical debt
        - The Splurge: a grab bag of various useful stuff: account abstraction, EVM improvements, PBS, etc.

### 2024.4.8

Ethereum design rationale(outdated but still worth learning)

1. Principles
    - **Sandwich complexity model**: the bottom level architecture of Ethereum should be as simple as possible, and the interfaces to Ethereum should be as easy to understand as possible. The "middle layers" of the protocol can be complex.
    - **Freedom**: net neutrality, no resstriction to use Ethereum protocol.
    - **Generalization**: protocol features and opcodes in Ethereum should embody maximally low-level concepts, so that they can be combined in arbitrary ways.
    - **We Have No Features**: often refuse to build in even very common high-level use cases
    - **Non-risk-aversion**

2. Blockchain-level protocol
    - Accounts and not UTXOs
        - benefit of UTXO: higher degree of privacy and potential scalability paradigms
        - benefits of accounts: large space savings, greater fungibility, simplicity and constant light client reference
    - Merkle Patricia Trees
        - Combination of Merkle tree and Patricia tree
        - Every unique set of key/value pairs maps uniquely to a root hash
        - It is possible to change, add or delete key/value pairs in logarithmic time
    - RLP
        - RLP ("recursive length prefix") encoding for serialization
        - (1) simplicity of implementation, and (2) guaranteed absolute byte-perfect consistency
    - Compression algorithm
    - Trie Usage
        - the state trie: representing the entire state after accessing the block
        - the transaction trie: representing all transactions in the block keyed by index
        - the receipt tree, representing the "receipts" corresponding to each transaction
    - Uncle incentivization
        - blockchains with fast confirmation times currently suffer from reduced security due to a high stale rate
        - To solve network security loss, it includes stale blocks in the calculation of which chain is the "longest"
        - To solve centralization bias, it provides block rewards to stales, 1/8
        - up to the seventh-generation descendant
    - Difficulty Update Algorithm
    - Gas and Fees
        - preventing denial-of-service attacks via infinite loops
    - Virtual Machine
        - Ethereum virtual machine: Simplicity, Total determinism, Space savings, Specialization to expected applications, Simple security, Optimization-friendliness
        - Temporary/permanent storage distinction
            - temporary storage: exists within each instance of the VM and disappears when VM execution finishes
            - permanent storage: exists on the blockchain state level on a per-account basis
            - purpose:
                1. provide each execution instance with its own memory that is not subject to corruption by recursive calls, making secure programming easier
                2. provide a form of memory which can be manipulated very quickly, as storage updates are necessarily slow due to the need to modify the trie.
        - Stack/memory model
        - 32 byte word size: enough but not inefficient
        - Having our own VM at all: allow to design simple VM and specialize VM, but not to have a very complex external dependency
        - Using a variable extendable memory size
        - Not having a stack size limit
        - Having a 1024 call depth limit
        - No types

### 2024.4.9

Merkle Patricia Trie

1. Ethereum uses 3 trie structures: stateRoot, transactionRoot, and receiptsRoot.
![image](https://github.com/qiwihui/intensive-ethereum-protocol-study-group/assets/3297411/61a00bf1-79ce-46f8-addb-756e546fb314)
    - state trie / world state trie
        - a mapping between account addresses and the account states including balance, nonce, codeHash, and storageRoot
        - the key is a 160 bit address of an Ethereum account
        - storageRoot: an account storage trie, the contract data associated with an account, the hash of the root node
    - Transaction trie
        - created on the list of transactions within a block, the path is tracked based on the position of the transaction within the block
        - once minted, never updated
        - transactionRoot is the hash of the root node
    - Receipt trie
        - the result of the transaction which is executed successfully
        - consists of four items: status code of the transaction, cumulative gas used, transaction logs, Bloom filter
        - the key is an index of transactions in the block
        - never get updated
        - receiptsRoot is the hash of the root node
2. The value in the trie often consists of multiple data items, Ethereum uses `Recursive Length Prefix` (RLP) encoding for data structures
3. Ethereum Merkle-Patricia Trie
![YZGxe (1)](https://github.com/qiwihui/intensive-ethereum-protocol-study-group/assets/3297411/e555a2ba-27e8-40d2-a32f-cb4b26c6672f)
4. An [example](https://ethereum.stackexchange.com/a/39918/91862) of inserting 3  key-value pairs into trie:

```text
"0x01": 1
"0x01234": 2
"0x01235": 3
```

after `0x01` is inserted, hash0 is root

```text
<hash0> leaf ["0x01", 1]
```

After `0x01234` is inserted, hash1 is root

```text
<hash1> extension ["0x01", <hash2>]
<hash2> branch [NULL,NULL,<hash3>,..<13 NULLs>.., 1]
<hash3> leaf ["0x34", 2]
```

After `0x01235` is inserted, hash4 is root:

```text
<hash4> extension ["0x01", <hash5>]
<hash5> branch [NULL,NULL,<hash6>,..<13 NULLs>.., 1]
<hash6> extension ["0x3", <hash7>]
<hash7> branch [NULL,NULL,NULL,NULL,<hash8>,<hash9>..<10 NULLs>.., NULL]
<hash8> leaf ["", 2]
<hash9> leaf ["", 3]
```

Generally, while inserting a key-value pair:

- if you stopped at a NULL node, you add a new leaf node with the remaining path and replace NULL with the hash of the new leaf.
- if you stopped at a leaf node, you need to convert it to an extension node and add a new branch and 1 or 2 leafs.
- if you stopped at an extension node, you convert it to another extension with shorter path and create a new branch and 1 or 2 leafs. If the new path turns out to be empty you convert it to a branch instead.

When deleting a key-value pair:

- if there is a branch that has a single non NULL nibble and NULL value, this branch can be replaced with a leaf or an extension.
- if there is an extension that points to another extension or a leaf, it can be collapsed into a single extension/leaf.
- if there is branch with all NULL nibbles and non NULL value, it can be converted into a leaf.

When adding/deleting key-value pairs the algorithm can make the decision locally at the current node, there is no need to create an unpacked version of the trie first and then pack it.

5. Why Merkle Patricia trie?
    - quick re-calculation of root hash when updates
    - easy detection of data changes
    - Querying is easy without requiring re-computation of the whole trie.

6. Merkle Vs Merkle-Patricia
    - Root of a Merkle Patricia trie does not depend on the order of data while Merkle root depends on.
    - Tree size of Merkle Patricia trie is lower than a standard Merkle tree due to prefix based compression techniques.
    - Merkle patricia tries are faster than Merkle trees, but the implementation is complicated.

### 2024.4.10

#### Next Generation ZKP System and Single Slot Finality

A prover, maybe malicious prover, makes a claim. The verifier does not know what the prover is going to claim.
To verify these claim, the verifier need some some to help him.

There are some ways to solve the problem.

case 1: The verifier leverages a trust the third party that can help you to understand the claim.

- trusted setup using groth16 or plonk
- require a lot of prover's resources and it also introduce additional trust
- extensive computation like MSM or FFT

case 2: preprocessing via polynomial commitment by verifier

- take a long time to preprocessing
- usually makes prover slower

case 3: compilation

- compiler identifies patterns in the source code of the circuit
- complier indicates the that the source code is simply repeating the sentence

#### A revolutionary compiler -- Log Complier

Log Complier， inspired from that the large circuit can be generated from a very small program

- Name from "log-space uniform circuit"
- Initially support Gnark's frontend language at launch
- Adding Plony, Halo, Circom later
- Open source: May 2024, together with the Prover

Overall architecture

- Using Gnark circuit language, discribed by go language
- Pass the circuit to GKR specialzed compiler
- Output specical circuit output, consisting of different subcircuits identified by compiler
- For the verifier to unstandand the circuit, it only need to look at each at individual subcircuit instead of whole circuit

Pros:

- the verifier can save the computation by orders of magnitude
- This new compiler potentially could enable a recursive snark because the verifier circuit is orders of magnitude smaller, allowing us to directly recurse and make everything infinitely recursive.

#### Next generation of ZKP

- 3M constraints per seconds per CPU core(BN254)
- GKR based

Prover structure

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/ab3022b4-08b4-4ce0-bcf1-bd47d34f4649)

- the end to end experience is like you write a circuit program and you execute it on the approver, the finally you can verify this proof on the Ethereum chain
- verify billion size circuit on chain
- papers related to these
  - [Libra: Succinct Zero-Knowledge Proofs with Optimal Prover Computation](https://eprint.iacr.org/2019/317)

#### Infinite horizontal scalability

zero-overhead distributed computing

scalability issue for the origin FRI:

- it relies on the FFT but FFT itself has a problem of distributed Computing. In the middle of fft there is a linear size of communication between machines
- For example: for the following circuit, FRI network communication scale serveral Gigabytes.
  - Sub-circuit size: 1M
  - number of sub-circuits: 256
- can only place the machines in the same data center.

Solution: Bi-variate KZG algorithm

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/fa54273b-79bb-41f1-97d4-e86ce0ab7ee2)

- Paper: [Pianist: Scalable zkRollups via Fully Distributed Zero-Knowledge Proofs](https://eprint.iacr.org/2023/1271)
- replace the FFT with scheme: Polynomial f(x,y) -> f(machine_id, y), which require constant communication between different machines
- O(1) communication between mahchines
  - concretely solution: GKR + Bi-KZG
  - each node needs to send 100 KB data
  - Most of them comes from GKR
- lead to global scale decentralization

#### Decentralization

performance:

- verifier 32768 signatures
  - time: 5~6 seconds
  - hardware requirements: 64 handheld machines
  - hardware requirement: stable network connection, mobile CPU, 16GB RAM

All validators in one slot

- fast finality
- improved scalability
- reduced network communication

current Ethereum's solution

- almost 1 million validators
- divided into 32 different slots, each has 30k validators
- problem: have to wait a lot of slot/blocks to get finality and get tx confirmed
- Ethereum: ~20 slots to get tx confirmed
- with more committees, the p2p network get overloaded

this signature verification algorithm can batch signatures into very small proofs instead of sending all the public keys, so to reduce the burden of the p2p network

#### A chatGPT summary

- **Verification Challenges and Current Solutions:**
  - Encounter with potentially malicious provers online, leading to verification challenges.
  - Current solutions involve reliance on trusted third parties or extensive personal computation.
- **Proposal of Compiler-based Verification:**
  - Introduction of a compiler-based solution to simplify verification by identifying patterns.
  - Compiler significantly reduces computational resources required for verification.
- **Introduction to Log Compiler:**
  - Tentative name for the revolutionary compiler, inspired by log space uniform circuits.
  - Initial support for Genox front-end language with plans for expansion.
- **Architecture and Benefits of the Compiler:**
  - Compiler's structure involves generating circuits with specific substructures for easier verification.
  - Potential for recursive snark due to significantly smaller verifier circuit.
- **Advancements in Proof Structure and Approver:**
  - State-of-the-art GKR-based prover capable of high-speed operation.
  - End-to-end experience: writing circuit program, execution on approver, and verification on the chain.
- **Addressing Scalability and Network Overload:**
  - Proposal for a signature verification algorithm to batch signatures into small proofs.
  - Reduces burden on P2P network, enabling single slot finality and alleviating network overload.

### 2024.4.11

1. transaction finality

A transaction has finality on Ethereum when it is part of a block that can’t change. also means block can’t change

2. Transaction Finality Under Proof-of-Work

There was always a potential for reorgs, so dapps would often wait several blocks before classifying a transaction as "confirmed" (i.e. unlikely to be removed from the canonical chain)

- the more block, the less possibility
- no real sense of time, just block number.

3. Transaction Finality Under Proof-of-Stake

- PoS introduced safe head (justified) blocks and finalized blocks, which can be used more reliably than "confirmed" PoW blocks finalized block can’t be reverted, strong startement, but it doesn’t mean is absolutely can’t be reverted. it means that the cost of reversion would be so extreme, one third of total staked ETH.
- there is an actual clock. every slot occurs ecery 12 seconds.
- you can confidently say a block has been finalized after 2 epochs.

![i](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/f3587931-65bc-4b53-8c01-c1b0023ac88b)

- slot: occur actually 12 seconds, there may be a block in a slot. missed slot contains no blocks.
- epoch: 32 slots, 6.4 minutes. basic unit to determine the level of finality of transactions.

- at the beginning of the epoch, the first slot is special, a checkpoint to say that if 2/3 of the staked ETH have been voted on this slot, then the previous epoch is justified. and the previous one epoch of the previous checkpoint is upgraded to finalized from justified.

- it is "justified," meaning under normal network conditions it is expect it to be included in the canonical chain and finalized.

two components to the consensus algorithm: Casper FFG and LMD Ghost.

- Casper FFG: take the aggregated stake of all the validators that attesting to a particular slot and particular epoch. once achieves 2/3, then a block can be justified or finalized.
- LMD Ghost: fork choice rule, designed to keep the timeline moving forward.
trade-off in PoS between liveness which is you keep building blocks, the chain doesn’t halt, and casper adding safety on top of liveness.

4. how are proposer and attesters chosen.(TODO)

### 2024.4.12

Ethereum nodes and clients
    - A "node" is any instance of Ethereum client software that is connected to other computers also running Ethereum software, forming a network.
    - A node has to run two clients: a consensus client and an execution client.
    - execution client
        - listens to new transactions broadcasted in the network
        - executes them in EVM
        - holds the latest state and database of all current Ethereum data.
    - consensus client, aka Beacon Node
        - implements the proof-of-stake consensus algorithm including the fork-choice algorithm, processing attestations and managing proof-of-stake rewards and penalties.
        - validator, allowing a node to participate in securing the network
    - client diversity: multiple client implementations
    - node types
        - full node
            - Stores full blockchain data
            - Participates in block validation, verifies all blocks and states.
            - All states can be either retrieved from local storage or regenerated from 'snapshots' by a full node.
            - Serves the network and provides data on request.
        - archive node:  ull nodes that verify every block from genesis and never delete any of the downloaded data.
        - light node: only download block headers, do not participate in consensus
    - execution clients: Geth, Nethermind, Besu, etc.
    - consensus clients: Prysm, Lighthouse, Teku, etc.
    - Synchronization modes
        - Execution layer sync mode
            - Full archive sync
            - Full snap sync
            - Light sync
        - Consensus layer sync mode
            - Optimistic sync
            - Checkpoint sync

### 2024.4.13

1. how are proposer and attesters chosen.

not every validators are voting or testing on every slot.
committee: at least 128 validators are chosen for a committee. A validator can only be in one committee per epoch.
All active validators are separated using RANDAO function into 32 committees in an epoch. Then for every slot, one validator is selected to be proposer to propose a block and the rest of the validators will be attesters for the slot.

![i](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/c7ff6c02-8f88-4893-8784-87c870e39ae7)

2. What happens in a slot?

slot has three distinct four-second phrases:

- t=0~4: Block propagation, Honest proposer broadcasts their block at the start of their slot
- t=4~8: Attestation aggregation, Attestation deadline, where attesters determine which block they will vote for based on the fork-choice rule. usig BLS signatures .
- t=8~12: Aggregation propagation, A subset of validators broadcast an aggregate,

![i](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/e00c9844-0002-415a-a549-2a80bb054725)

3. Consensus layer

validators: responsible for proposing new blocks, validating existing ones, and processing transactions.

### 2024.4.14

1. Ethereum: mechanics

beacon chain:

- becoming a validator: 32 ETH or use Lido.
- validators:
  - sign blocks to express correctness
  - occasionally act as block proposer
  - correct behavior -> issued new ETH
  - incorrect behavior -> slashed

2. Ethereum compute layer: the EVM

- accounts:

|Account data| EOA | Contacts |
|--|--| -- |
|address|H(PK)| H(creatorAddress+creatorNonce)|
|code|-|CodeHash|
|storage root(state)|-|storageRoot|
|balance|balance|balance|
|nonce|nonce|nonce|

- Account state:
  - storage array S[]: S[0], S[1], … , S[2^256-1]:
  - Account storage root: Merkle Patricia Tree hash of s[]
    - time to compute root hash: <= 2 * |S|, |S| = # non-zero cells
    - for example
    ![Screenshot 2024-04-14 at 17 37 42](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/969c6222-65b9-4ea0-b2ec-b9e7c276650e)
- State transitions
  - To: 32-byte address of target (0 -> create new account)
  - From, [Signature]: initiator address and signature on Tx (if owned)
  - Value: # Wei being sent with Tx (1 Wei = 10^-18 ETH)
  - Tx fees (EIP 1559): gasLimit, maxFee, maxPriorityFee (later)
  - if To = 0: create new contract `code = (init, body)`
  - if To ≠ 0: data (what function to call & arguments)
  - nonce: must match current nonce of sender (prevents Tx replay)
  - chain_id: ensures Tx can only be submitted to the intended chain
- transaction types:
  - owned -> owned: transfer ETH between users
  - owned -> contract: call contract with ETH & data
- virtual Tx initiated by a contract
  - contract -> owned: contract sends funds to user
  - contract -> contract: one program calls another (and sends funds)
- An Ethereum Block
  - Block proposer produce a block do:
    1. for i=1,…,n: execute state change of Tx_i sequentially
    2. record updated world state in block
  - Other validators re-execute all Tx to verify block
- Block header data
  - consensus data: proposer ID, parent hash, votes, etc.
  - address of gas beneficiary: where Tx fees will go
  - **world state root**: updated world state, Merkle Patricia Tree hash of all accounts in the system
  - **Tx root**: Merkle hash of all Tx processed in block
  - **Tx receipt root**: Merkle hash of log messages generated in block
  - Gas used: used to adjust gas price (target 15M gas per block)
- EVM mechanics: execution environment
  - Write code in Solidity
  - compile to EVM bytecode
  - validators use the EVM to execute contract bytecode
- The EVM
  - Stack machine, max stack depth = 1024
  - program aborts if stack size exceeded
  - two types of zero initialized memory
    - Persistent storage (on blockchain): `SLOAD`, `SSTORE` (expensive)
    - Volatile memory (for single Tx): `MLOAD`, `MSTORE` (cheap)
    - `LOG0`(data): write data to log
  - Every instruction costs gas, some gets gas refund
  - Why charge gas?
    - prevents submitting Tx that runs for many steps(DoS attack)
    - block proposer chooses Tx from mempool that maximize its income.
  - Gas calculation: EIP1559
    - goals:
      - users incentivized to bid their true utility for posting Tx,
      - block proposer incentivized to not create fake Tx, and
      - disincentivize off chain agreements.
    - Every block has a `baseFee`:
      - the minimum gasPrice for all Tx in the block
      - computed from total gas in earlier blocks
        - earlier blocks at gas limit (30M gas) -> base fee goes up 12.5%
        - earlier blocks empty -> base fee decreases by 12.5%
    - Three parameters:
      - `gasLimit`: max total gas allowed for Tx
      - `maxFee`: maximum allowed gas price
      - `maxPriorityFee`: additional "tip" to be paid to block proposer
      - Computed gasPrice bid: `gasPrice <- min(maxFee, baseFee + maxPriorityFee)`
      - Max Tx fee: `gasLimit × gasPrice`
    - gasUsed ⇽ gas used by Tx
      - Send `gasUsed×(gasPrice – baseFee)` to block proposer
      - BURN `gasUsed× baseFee`
      - total supply of ETH can decrease
    ![i](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/2a5a3dfc-337a-4a68-bf23-ca5dfcc0c185)
    - Why burn ETH?
      - Suppose no burn (i.e., baseFee given to block producer) => in periods of low Tx volume proposer would try to increase volume by offering to refund the baseFee off chain to users.
