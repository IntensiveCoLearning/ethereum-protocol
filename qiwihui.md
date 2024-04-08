# qiwihui

Hi, I am qiwihui, a python dev. I am interested in Ethereum protocol. Looking forward to learning from you guys.

- Twitter/X: https://twitter.com/qiwihuix
- Telegram: https://t.me/qiwihui

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
