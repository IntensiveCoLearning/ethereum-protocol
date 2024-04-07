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
