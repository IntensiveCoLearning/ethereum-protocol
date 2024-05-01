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

- [Single Slot Finality](https://ethresear.ch/t/single-slot-finality/16700)

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
  - implements the proof-of-stake consensus algorithm including the fork-choice algorithm, processing attestations and managing proof-of-stake  rewards and penalties.
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

### 2024.4.15

the execution layer node

- Block validation
  - ELs process state transaction
  - each transaction is validated by the client, executed, and its result accumulated into the state trie
  - add additional mechanisms to blocks:
    - such as the EIP-1559 base fee, the EIP-4844 excess blob gas, the EIP-4844 beacon root ring buffer, beacon chain withdrawals
- Block building
  - build blocks
  - sync transaction pool over p2p
- State transition function
  - header validation
    - verify merkle roots
    - verify gas limit
    - verify timestamp
  - block validation

### 2024.4.16

1. block validation

- `process_execution_layer` by beacon chain
  - call `verify_and_notify_new_playload`: verify whether a block is valid and move the CL forward
  - `notify_new_playload`: send payload to execution engine
- simplified state transaction function

```go
func stf(parent types.Block, block types.Block, state state.StateDB) (state.StateDB, error) {
    if err := core.VerifyHeaders(parent, block); err != nil {
            // header error detected
            return nil, err
    }
    for , tx := range block.Transactions() {
        // block header info is to be access RANDAO value to gain some randomness, or base fee in the header
        res, err := vm.Run(block.Header(), tx, state)
        if err != nil {
                // transaction invalid, block is invalid
                return nil, err
        }
        state = res
    }
    return state, nil
}

// will be call from beacon chain, if block is not valid, it will be rejected
func newPayload(execPayload engine.ExecutionPayload) bool {
    if , err := stf(..); err != nil {
        return false
    }
    return true
}
```

- Verify the headers
  - Error could happen if
    - The gas limit change exceeds 1/1024th of the previous block's
    - Block numbers are not sequential
    - EIP-1559 base fee is not updated correctly
    - etc.

### 2024.4.17

1. block building

```go
func build(env Environment, pool txpool.Pool, state state.StateDB) (types.Block, state.StateDB, error) {
    var (
        gasUsed = 0
        txs []types.Transactions
    )
    for ; gasUsed < env.GasLimit || !pool.Empty(); {
        tx := pool.Pop()
        res, gas, err := vm.Run(env, tx, state)
        if err != nil {
            // tx invalid
            continue
        }
        gasUsed += gas
        txs = append(txs, tx)
        state = res
    }
    return core.Finalize(env, txs, state)
}
```

- Parameters needed
  - Environment: including Timestamps, block number, previous block, base fee, etc.
  - Tx pool: Maintain the list of txns, ordered by their value (most value per gas)
  - StateDB
- Return results
  - Block
  - Updated stateDB
  - Error
- Steps:
  1. Track gas used and store txs going to the block: 30M for mainnet
  2. Get the next best tx from the tx pool and execute it: skip if invalid
  3. Use Finalize function to return results, a fully assembled block

### 2024.4.18

1. state transition function

- walk through go-ethereum
  - Function newPayload
  - Function insertBlockWithoutSetHead
  - Function insertChain
  - Function Process

2. EVM

- evm structure
  ![output](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/e9f6c53f-ef1d-4765-962b-6f17217b0ee9)
- Different types of instructions within the EVM: see <https://www.evm.codes>

3. p2p

- The EL operates on devp2p
- Responsibility of the p2p protocol: Access to historical data & pending txs, and the state

### 2024.04.20

[分布式系统](https://mp.weixin.qq.com/s/DyaFZ05HzY84vRi2Ys90Dg)

1. 区块链是一种分布式系统：为了解决特定的问题，在特定的环境中，做出特定的解决方案。可以让我们免除对中心化的那一台计算机，以及那台计算机背后的中心化的公司或组织的依赖。
2. 分布式系统的理想目标

- 区块链所属的分布式系统——复制状态机模型： 统内所有的节点 / 计算机都有相同的初始状态，在执行完一个事务后，所有的节点都有相同的最终状态。
- 分布式系统「FLP 不可能原理」：在网络可靠，但允许节点失效的最小化异步模型系统中，不存在一个可以解决一致性问题的确定性共识算法。
- FLP 不可能原理告诉我们：不要浪费时间去为分布式系统设计面向所有场景的共识算法，那是不可能实现的。

3. 分布式系统的共识算法

- 寻找在特定场景中有效的共识算法。
- 首先，确定共识的到底是什么：
  - 最简单的方法就是某一台计算机说了算，它提出一个结果，其他的计算机来表态是否同意这个结果。
  - 确定一个中心，并关注选出这个领导者的方法是否去中心化。
- 需要领导者的共识算法的工作步骤
  1. 选出一个领导者；
  2. 领导者提出一个结果；
  3. 追随者确定是否同意这个结果；
  4. 如果大家就结果达成了共识，系统输出最终结果；如果大家未达成共识，回到步骤 1 重新开始。

4. 同步性假设共识算法

- 如果网络中有计算机连不上系统怎么办？如果一台计算机连不上系统，就忽略它，不要它参与这一轮的共识。但是怎么知道是连不上还是比较慢？
- 同步性假设： 引入「超时」概念，也就是说事先设定一个时间范围，如果领导者无法在该时间范围内发出提案，就淘汰它，选出一个新的领导者。
- Paxos 算法和 Raft 算法都是基于同步性假设提出来的。
  - 还对系统做另一种假设，即认为系统内所有的计算机都是「好人」，它们要么正确地响应领导者的提案，要么因为故障无法响应。
  - 制定一条规则：只要系统内过半数的计算机接受了领导者的提案，就把该提案作为系统的最终结果。

5. 解决掉系统中的「坏人」

- 拜占庭将军问题： 其中的拜占庭节点就是坏人节点，它们会传递干扰信息阻碍整个系统达成共识。
  - 兰伯特提出了几种解决方案，其中一种可以在拜占庭节点不到 1/3 时实现系统的共识。
  - DLS 算法、PBFT 算法（实用拜占庭容错算法）在此基础上发展出来。
- PBFT
  1. pre-prepare 阶段：领导者发送结果给所有追随者。领导者在本图中是 0 号节点，它把结果发给追随者 1、2、3 号节点。
  2. prepare 阶段：如果追随者认为结果没有错误，就告诉所有其他节点自己认可这个结果。比如 1 号节点会把自己的认可消息发给 0、2、3 号节点。
  3. commit 阶段：如果追随者发现超过 2/3 的节点认可了领导者的结果，就告诉所有其他节点自己接受这个结果为最终结果。
  4. reply 阶段：如果领导者和追随者发现超过 2/3 的节点接受了最终结果，就可以认为大部分节点达成了共识，就把该共识反馈给客户端；如果客户端收到超过 1/3 的节点的相同的共识，就可以认为全网达成了共识。
  ![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/b4a11592-718e-43fd-b681-f1f51c1315ef)
- 不过如果系统中坏人的数量等于或多于 1/3，依然是无法达成共识的。

6. 中本聪共识算法

- 中本聪共识使用的是非确定性机制
- 两种选择： 如果要求结果确定，就不能保证一定能等到结果；如果要求拿到结果，就无法保证该结果一定是最终结果。
- 分布式系统就是这样，只能二选一，第一种选择被称作 Finality，即「结果的确定性」或安全性；第二种选择被称作 Liveness，即网络的活性或可用性。
- 这两种选择决定了分布式共识两种不同的设计思路：
  - 追求 Finality，是优先结果，就要对网络做出要求。
    - PBFT、Tendermint 都是这一类型的算法，它们走的是网络的同步性假设路线，使用这类算法的系统不会出现分叉。
  - 追求 Liveness，是优先网络，就要对结果做出让步。
    - 中本聪共识是这一类型的算法，它走的是结果的非确定性路线，使用这类算法的分布式网络始终可用，而且任意节点都可以随时加入 / 离开系统。
- 在 Finality 和 Liveness 中二选一也是分布式系统 CAP 定理（不可能三角）的体现
  - 分布式系统 CAP 定理（不可能三角）： 对于一个分布式系统来说，不可能同时满足一致性、可用性和分区容错性。
  - 因为分区容错性是指该系统要能容忍网络出现分区，而现实网络是一定会分区的，所以这个条件必须满足
  - CAP 定理说的是一个分布式系统不可能同时满足一致性和可用性，这其中，CAP 一致性体现的是 Finality，CAP 可用性体现的是 Liveness。
- 使用非确定性机制的中本聪共识描述起来也很简单：如果你看到某提议的区块拥有最多的工作量证明，就接受该区块，这也被称作最长链规则。
- 并非所有的 PoS 都是 Finality 路线，比如 Casper FFG 就不是；而 PoW 也不是只能走 Liveness 路线。
- 中本聪共识追求的是 Liveness
- Finality 系统在保证了结果的确定性后，系统设计就要反过来追求 Liveness；而 Liveness 系统在保证了网络的开放性后，系统设计就要反过来追求 Finality。
  - 中本聪共识为了提高结果的确定性或安全性，就需要做出其他让步，比如 TPS。

7. 总结

- 两个定理：FLP 不可能原理；CAP 不可能定理。
- 两种容错能力：宕机容错；拜占庭容错。
- 两种共识算法设计思路：Finality；Liveness。
- 两类共识算法：同步性假设；非确定性机制。
- 三个共识算法：Paxos、PBFT、中本聪共识。

8. further reading

- [Let’s take a crack at understanding distributed consensus](https://www.preethikasireddy.com/post/lets-take-a-crack-at-understanding-distributed-consensus)
- [WHAT WE TALK ABOUT WHEN WE TALK ABOUT DISTRIBUTED SYSTEMS](http://alvaro-videla.com/2015/12/learning-about-distributed-systems.html)
- Time, Clocks and the Ordering of Events in a Distributed System

### 2024.04.21

[The Beacon Chain](https://ethos.dev/beacon-chain)

1. Slots and Epochs

- Each slot is 12 seconds and an epoch is 32 slots: 6.4 minutes.
- Every 12 seconds, one block is added when the system is running optimally.
- A slot is like the block time, but slots can be empty.

2. Validators and Attestations

- An attestation is a validator’s vote, weighted by the validator’s balance.
- Each validator has a maximum balance of 32 ETH
- One validator client can execute many validators.

3. Committees

- RANDAO
- Attesting to the Beacon Chain head is called an LMD GHOST vote.

4. Beacon Chain Checkpoints

- Only validators assigned to a slot cast an LMD GHOST vote for that slot. However, all validators cast FFG votes for each epoch checkpoint.
- Supermajority: A vote that is made by 2/3 of the total balance of all active validators, is deemed a supermajority.

5. finality

- justified, finalized
- The epoch boundary block at Slot 96 is proposed and contains attestations for the Epoch 2 checkpoint.

6. Attestations

- An attestation contains both an LMD GHOST vote and an FFG vote
- Validators are rewarded the most when their attestation is included on-chain at their assigned slot; later inclusion is a decaying reward.

TODO

7. Staking Rewards and Penalties
8. Slashable Offences
9. Beacon Chain Validator Activation and Lifecycle

### 2024.04.22

7. Staking Rewards and Penalties

- attester rewards
  - LMD GHOST and FFG votes
  - Attestations in finalized blocks are worth more
- attester penalties
  - not attesting or if they attest to blocks that are not finalized.
- typical downside risk for stakers
- slashings and whistleblower rewards
  - 0.5 ETH up to a validator’s entire stake
- proposer rewards
- inactivity leak penalty

8. Slashable Offences

- four slashing conditions for validators
  - a double proposal: proposing more than one block for their assigned slot.
  - an LMD GHOST double vote: attesting to two different Beacon Chain heads for their assigned slot.
  - an FFG surround vote: casting an FFG vote that surrounds or is surrounded by a previous FFG vote they made.
  - an FFG double vote: casting 2 FFG votes for any two targets at the same epoch.

9. Beacon Chain Validator Activation and Lifecycle

![i](https://ethos.dev/assets/images/posts/beacon-chain/Beacon-Chain-Validator-Lifecycle.png.webp)

### 2024.04.23

Execution layer testing

EVM testing

- import characterics of testing
  - pre-state
  - environment
  - transaction(s)
  - post-state

EVM testing - tests filling

- Process of compiling a test source code into a fixture that can be consumed by any execution client
- For test filling, the exact same test can be executed in any client implementation.
- json

EVM testing formats

- State testing: Use the state root for verification
- Fuzzy differential state testing: fuzzed smart contract code
- Blockchain testing
- Blockchain negative testing
  - Add an invalid block at some point to check if the clients can reject the invalid block for the designed purpose, go back to the previous valid block and claim it as the chain head

Tests Filling in details

- ethereum/tests
- ethereum/execution-spec-tests
- FuzzyVM
- Execution APIs testing

Consensus Layer Testing

Cross-Layer (Interop) Testing

Devnets
Shawdow forks
Public Testnets

### 2024.04.24

Security

- EL sides
- CL sides

Research and Roadmap overview

- The Merge: upgrades relating to the switch from proof-of-work to proof-of-stake
  - beacon chain
  - Sync committee / Light client protocol
  - Secret Leader Election <- EIP-7441
  - Single Slot Finality
    - goal
      - From 12.6 minutes to 12 seconds
      - Main problem: Too many signatures to check and aggregate
    - Solution paths:
      - Fewer validators (MaxEB)
      - Fewer active validators
      - Way fewer validators (8192) + Distributed Validators Tech
      - Better signature aggregation schemes
    - Quantum-proof Beacon Chain
      - STARKs
- The Surge: upgrades related to scalability by rollups and data sharding
  - zk/op rollups
  - KZG Ceremony
  - EIP-4844
  - Data Availability Sampling
  - Cross-rollup interop
- The Scourge: upgrades related to censorship resistance, decentralization and protocol risks from MEV
  - (very) brief overview of MEV
  - ePBS / Inclusion Lists / MEV Burn
  - Max EB, Stake capping
- The Verge: upgrades related to verifying blocks more easily
  - Verkle Trees
  - SNARKify everything:
    - Beacon fast sync
    - Beacon state transition
    - Verkle proofs
    - EVM
- The Purge: upgrades related to reducing the computational costs of running nodes and simplifying the protocol
  - EIP-4444
  - Protocol simplifications
- The Splurge: other upgrades that don't fit well into the previous categories.
  - EIP-1559 endgame
  - Account Abstraction
  - Deep cryptography

### 2024.04.26

Verkle trie (with the KZG commitment scheme)

#### KZG polynomial commitments

1. Summary

- Kate，Zaverucha 和 Goldberg
- 在一个多项式承诺方案中，证明者计算一个多项式的承诺（commitment）, 并可以在多项式的任意一个点进行打开（opening）：该承诺方案能证明多项式在特定位置的值与指定的数值一致。
- 之所以被称为承诺，是因为当一个承诺值（椭圆曲线上的一个点）发送给某对象（ 验证者），证明者不可以改变当前计算的多项式。它们只能够对一个多项式提供有效的证明；当试图作弊时，它们要不无法提供证明，要不证明被验证者拒绝。

2. 默克尔树对比

- 矢量承诺：运用一个深度为 $d$ 的默克尔树，你可以计算一个矢量的承诺。用 $d$ 个哈希来提供证明元素 $a_i$ 存在于这个矢量的位置 $i$。
- 用默克尔树来构造多项式承诺： 通过设置 $a_i=p_i$ ，我们可以计算这一系列系数的默克尔树根，从而比较容易地对一个 $n=2^d−1$ 次的多项式进行承诺，其中 $p_i$ 是 $n$ 次多项式 $p(X)$ 的系数。
- 证明一个取值，意味着证明者想要向验证者展示对于某个值z，$p(z)=y$。为达到这个目的，证明者可以向验证者发送所有的 $p_i$，然后验证者计算 $p(z)$ 是否等于y。
- 多项式承诺的性质：
  - 承诺的大小是一个单一哈希（默克尔树根）。一个足够安全的加密散列一般需要256位，即32字节。
  - 为了证明一个取值，证明者需要发送所有的 $p_i$，所以证明的大小和多项式次数是线性相关的。同时，验证者需要做同等的线性量级的计算
  - 该方案不隐藏多项式的任何部分
- KZG 承诺
  - 承诺大小是一个支持配对的椭圆曲线群元素。比如说对于BLS12_381曲线，大小应是48字节。
  - 证明大小独立于多项式大小，永远是一个群元素。验证，同样独立于多项式大小，无论多项式次数为多少都只要两次群乘法和两次配对。大多数时候该方案隐藏多项式。

3. 椭圆曲线以及配对

- 可信设置： 通常采用安全多方计算（MPC），使用一组计算机来创建这个群元素，而没有任何单一计算机知道秘密s，这样只有挟持了整组计算机才能知道s。

4. KZG 承诺

没看懂 :(

![image](https://github.com/brucexu-eth/intensive-ethereum-protocol-study-group/assets/3297411/9dda212e-7f55-49a4-b3f9-ca1e38d0769b)

### 2024.04.27

EIP-3074 简介

- EIP 3074：操作码 AUTH 和 AUTHCALL
- EIP 3074 后，普通账户（EOA）可发送批量事务、限期事务、无序事务等
- 要想使用这两个操作码，外部账户需要在链下签署一个消息，并将该消息发送给中继者，再由中继者将签名和调用数据发送至一个链上合约（称为 “调用者”）。调用者合约会先使用操作码 AUTH 来验证签名，再使用操作码 AUTHCALL 中继外部账户的调用。
- AUTHCALL 与普通调用只有一个区别：AUTHCALL 将调用者（例如，消息发送方）设为使用操作码 AUTH 恢复的外部地址。
- EIP 3074 旨在淘汰元事务，降低合约的复杂性。

目的：

- 我们想要构建一个让普通用户无需使用以太币即可以免信任方式发送事务的机制。这里的关键词是 “免信任”，即，用户不会授予中继者任何可能会被利用的特权。
- EIP 3074 通过谨慎选择普通账户签名中包含的参数来创建免信任系统。用户签署 keccak（0x03 ++ invoker_address ++ commit_hash）。

![image](https://img.learnblockchain.cn/2021/05/21/16215616212581.jpg)

- “type byte” 是 EIP 2718 的常量字节，值为 0x03。这个字节的作用是避免与其它签名机制发生冲突，例如，EIP 2930 的访问列表事务、EIP 1559 的费用市场事务、EIP 191 的 0x19 签名消息等。

- 调用者地址将用户的调用与特定合约绑定。用户的签名只对调用者合约有效。因此，用户可以选择自己信任的调用者，就像是选择用来存放资产的智能合约钱包那样。
- 最后一个签名参数是 commit_hash（或者 commit）。这个 commit 限制调用者只能执行特定操作并创建特定的验证要求（validity requirement）来处理调用。用户可以信任调用者会遵循这一流程，因为他们可以在链上验证代码。

![](https://img.learnblockchain.cn/2021/05/21/16215616331751.jpg)

![](https://img.learnblockchain.cn/2021/05/21/16215616473335.jpg)

### 2024.04.29

[EIP-4844 背景与技术解读](https://learnblockchain.cn/article/7586)

1. EIP-4844

- 引入新的数据存储类型“blob”， 为 Layer 2 扩容方案提供更低的 DA 方案。
- 引入新的交易格式“blob-carrying transactions”，EVM 无需访问 blob 数据本身，仅通过其承诺（commitment）即可验证 blob 数据的正确性和完整性。

2. 背景

- Rollups 的相对昂贵源于 calldata 有限的存储空间和 Layer2 数据对 Layer1 较高的存储需求。
- 解决 Rollups 本身长期不足的长期解决方案一直是数据分片，然而，数据分片仍需要相当长的时间才能完成实施和部署。
- EIP-4844： "Proto-Danksharding"，实现将在完全分片中使用的交易格式，先行解决数据存储和传输的效率问题。

3. 技术细节

- 数据存储格式 blob
  - 字节向量（`ByteVector[n]`），`n = FIELD_ELEMENTS_PER_BLOB * BYTES_PER_FIELD_ELEMENT`。
    - `FIELD_ELEMENTS_PER_BLOB`：新增常量，表示每个 blob 中的字段数，为固定值 `4096`。
    - `BYTES_PER_FIELD_ELEMENT`：新增常量，表示每一个 blob 字段的存储字节数，为固定值 32。
  - 一个 blob 的可用容量为 4096 * 32 个字节，约为 0.125 MB 。
  - `MIN_EPOCHS_FOR_BLOB_SIDECARS_REQUESTS`： 为固定值 4096 ，定义了节点必须存储 blob 数据的最少 epoch 数（`4096 * 32 * 12 / 3600 / 24 = ~18.2 天`）
    - 期间数据依然可以被网络中的节点访问和验证；
    - 逾期后，节点有权删除 blob 中的 sidecars 数据
  - blob 数据的组成部分：
    - 用户数据： 要在以太坊上存储和传输的实际数据集
    - 数据承诺与证明： 确保数据的完整性和可验证性
- blob 交易
  - 符合 EIP-2718 交易框架
    - TransactionType || TransactionPayload 为有效的交易。
    - TransactionType || ReceiptPayload 为有效的交易收据。
  - blob 交易的 TransactionType 为 BLOB_TX_TYPE，其值为 `Bytes1(0x03)`。
  - blob 交易的 TransactionPayload 是 `TransactionPayloadBody` 的 RLP 序列化结果

    ```shell
    TransactionPayloadBody = [chain_id, nonce, max_priority_fee_per_gas, max_fee_per_gas, gas_limit, to, value, data, access_list, max_fee_per_blob_gas, blob_versioned_hashes, y_parity, r, s]
    TransactionPayload = rlp(TransactionPayloadBody)
    ```

  - blob 交易 TransactionPayloadBody 各字段解释如下：
    - blob 交易常规字段（与 EIP-1559 的语义相同）：
      - `chain_id`：链 ID 。
      - `nonce`：发送者账户的交易计数器。
      - `max_priority_fee_per_gas`：最大优先费用（小费）。
      - `max_fee_per_gas`：最大总费用（包括基础费用）每单位 gas 。
      - `gas_limit`：交易可使用的最大gas 量。
      - `value`：以 wei 为单位的发送的以太币数量。
      - `data`：交易的输入数据。
      - `access_list`：一个访问列表，包括交易执行过程中需要访问的地址和存储键，以优化 gas 消耗和提高交易执行效率。
    - blob 交易非常规字段（与 EIP-1559 的语义不同）：
      - `to` ：接收方的 20 字节地址，不同于 EIP-1559，但它不能为 nil。这意味着 blob 交易不能用来创建合约。
    - blob 交易独有字段：
      - `max_fee_per_blob_gas` ：uint256，用户指定的愿意给出的每单位 blob gas 的最大费用。
      - `blob_versioned_hashes`：方法 `{kzg_to_versioned_hash}` 的哈希输出列表，即一个数组，一个区块用到几个 blob 数组内就会有几个元素，每个元素为一个 blob 的 VersionedHash （即版本化哈希值，对 blob 的数据进行承诺，由方法 `{kzg_to_versioned_hash}` 基于 KZG 承诺生成。
  - blob 交易的 `ReceiptPayload`（在 EIP-2718 中定义） 为 `rlp([status, cumulative_transaction_gas_used, logs_bloom, logs]`) 。
    - `status`：交易执行的状态码（成功或失败）。
    - `cumulative_transaction_gas_used`：执行到当前交易为止累计的 gas 消耗量。
    - `logs_bloom`：交易产生的日志的Bloom过滤器，用于快速检索日志事件。
    - `logs`：交易执行过程中产生的日志事件列表。

- blob 交易的签名

blob 交易的发送者对交易数据进行的数字签名，先将 `BLOB_TX_TYPE` 与 `TransactionPayload` 的拼接结果作为消息原文做 Keccak256 哈希处理，获得摘要如下：

```shell
keccak256(BLOB_TX_TYPE || rlp([chain_id, nonce, max_priority_fee_per_gas, max_fee_per_gas, gas_limit, to, value, data, access_list, max_fee_per_blob_gas, blob_versioned_hashes]))
```

再将摘要进行 secp256k1 签名 获得签名值 `y_parity`、`r` 和 `s`。

- blob gas 设计
  - 引入 blob gas 作为一种新型 gas。它独立于普通 gas 并遵循自己的目标规则。
  - 单个 blob 的 gas 容量： `GAS_PER_BLOB`
  - 目标 blob gas 消耗量： `TARGET_BLOB_GAS_PER_BLOCK`， （3 * `GAS_PER_BLOB`）
  - 最大 blob gas 消耗量： `MAX_BLOB_GAS_PER_BLOCK`， （6 * `GAS_PER_BLOB`）
- blob 基础费更新规则
  - `blob_base_fee = MIN_BLOB_BASE_FEE * e**(excess_blob_gas / BLOB_BASE_FEE_UPDATE_FRACTION)`
- 区块头扩展： 区块头将被加入 2 个新字段
  - `blob_gas_used` 是区块内交易消耗的 blob gas 总量（即处理 blob 数据所需的 gas 量）
  - `excess_blob_gas` 是在出块之前，累计超出目标 blob gas 消耗的总量。
  - 区块头 RLP 编码也将包含这 2 个新字段。
- 获取版本化哈希值的操作码
  - 与 blob 数据哈希计算相关的新增常量：
    - HASH_OPCODE_BYTE：对 blob 数据哈希计算的操作码，为固定值 bytes1(0x49)。
    - HASH_OPCODE_GAS：blob 数据哈希计算所对应的 gas 消耗量，为固定值 3 。
  - 此 EIP 添加一条指令 BLOBHASH
- 点评估预编译
  - 用于验证一个数据集对应的KZG 承诺正确性的证明（这确保了数据的接收方可以验证数据的发送方实际上承诺了特定的数据集，而无需接收整个数据集本身）
  - 这个 KZG Proof 声称 blob（由承诺表示）在给定点（given point）评估为给定值（given value），即某个多项式 p(x)（由一个承诺表示）在给定点 z 上的值 p(z) 等于 y 。

```py
def point_evaluation_precompile(input: Bytes) -> Bytes:
    """
    Verify p(z) = y given commitment that corresponds to the polynomial p(x) and a KZG proof.
    Also verify that the provided commitment matches the provided versioned_hash.
    """
```

### 2024.04.30

review Modular Arithmetic, Finite Field of order prime p, Group, Generator of a Group and Cryptographic Assumptions.

link: <https://thogiti.github.io/2024/03/22/Mastering-KZG-by-hands.html>

### 2024.05.01

Verkle tries

- introduction
  - at each node, instead of a hash of the d nodes below (d=2 for binary Merkle trees), they commit to the $d$ nodes below using a vector commitment
  - d-ary Merkle trees: needs $(d-1)log_{d}{n}$ hashes
  - binary Merkle tree: $log_{}{n}$
  - by using KZG polynomial commitment scheme,  each level only requires a constant size proof

- A verkle trie is a trie where the inner nodes are d-ary vector commitments to their children, where the i-th child contains all nodes with the prefix i as a d-digit binary number.

- example of d=16

![](https://dankradfeist.de/assets/verkle_trie.svg)

The root of a leaf node is simply a hash of the (key, value) pair of 32 byte strings, whereas the root of an inner node is the hash of the vector commitment (in KZG, this is a $G_1$ element).

- Verkle proof for a single leaf
  - for each inner node that the key path crosses, we have to add the commitment to that node to the proof.
  - key and value are known, and `Root` is known to verifier(green), so for path in cyan color, we have to give the commitment to `Node A` and `Node B`
  - The `Root` as well as the node itself are marked in green, as they are data that is required for the proof, but is assumed as given and thus is not part of the proof.
  - consist of three KZG evaluation proofs
  - KZG proofs can be compressed using different schemes to a small constant size
- verkle vs. merkle
  - much more efficient in proof size
  - Merkle-Patricia Trie: 树上的一个节点，要么是 (i) 空的，要么是 (ii) 一个叶子节点，包含一个键和对应的值，或者是 (iii) 一个中间节点，拥有固定数量个子节点（这个数量也即是树的 “宽度”）
  - shorter proofs the higher the width

- Commitments and proofs
  - In a Merkle tree (including Merkle Patricia trees), the proof of a value consists of the entire set of sister nodes at each level.
  - In a Verkle tree, you just provide the path, with a little bit extra as a proof.
  - the hash function used to compute an inner node from its children is not a regular hash. Instead, it's a **vector commitment**.

![image](https://vitalik.eth.limo/images/verkle-files/verkle3.png)

In practice, we use a primitive even more powerful than a vector commitment, called a **polynomial commitment**.

- Polynomial commitments let you hash a polynomial, and make a proof for the evaluation of the hashed polynomial at any point
- use polynomial commitments as vector commitments: if we agree on a set of standardized coordinates $(c_1, c_2, ..., c_n)$, given a list $(y_1, y_2, ..., y_n)$, you can commit to the polynomial $P$ where $P(c_i)=y_i$ for all $i \in [1...n]$
 (you can find this polynomial with Lagrange interpolation).
- The two polynomial commitment schemes that are the easiest to use are KZG commitments and bulletproof-style commitments.
- if you use a KZG commitment and proof, the proof size is 96 bytes per intermediate node, nearly 3x more space-efficient than a simple Merkle proof if we set width = 256.
