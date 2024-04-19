# Steven

Hi, my name is Steven, studying and working in data. Looking forward to learning a lot from https://epf.wiki/#/ and meeting new amazing people. 
X/tele/ig: @im24steven

Timezone: PST

# Notes


## The Protocol: Data Structures

Regarding my original BG, I will be looking at what's the detailed, optimized data structure of ETH, e.g., what's in each trie that got hashed into the Merkle root within each block. Further, how different kinds of databases can be built.  

### 04/05 MPT

[Reference](https://epf.wiki/#/wiki/protocol/data-structures?id=data-structures-in-ethereum)



(Delving into the data structures of eth for a better understanding of building relational databases for eth transactions.)

1. Merkle Patricia Trie as the primary data structure for storing the execution layer state. PT: all data is stored in the leaf nodes. Space efficient.

	3 types of nodes in ETH MPT:
	
	- Branch Nodes: 1 node value + 16 branches = 17-element array. 
	- Extension Nodes: optimized, compressed, eliminating redundant branch nodes with single children.
	- Leaf Nodes: key-value pairs. Value = node content; Key = node hash.

![MPT](https://github.com/WildcatsC/eth-projects/blob/main/assets-pics/ETH-MPT.jpg)
(The root is also an extension node)

2. Within one ETH block, 4 different modified MPTs:

 	- **Transaction Trie:** Nonce, from, to, value, etc.
	- **Receipt Trie:** logs, status code, etc.
	- **World State Trie:** nonce, balance, storageRoot, etc.
	- **Account State(Storage) Trie:** storageRoot

([Visualization](https://epf.wiki/#/wiki/protocol/data-structures))

Insight: 1. Efficient data retrieval in MPT. 2. translating MPT into relational schemas, different parts of the Merkle tree can be stored in different tables. For building/saving historical data as relational databases using schemas with certain normalization levels, tables can be stored separately--nodes table, accounts state table, transactions table, etc. 

3. TODO: On Merkle Trees in ETH and ZKP: how ZKP project tables could be generated?

### 04/06 MPT

1. More MPT: ETH vs ZKP

	Iden3 as an example:

	Store & validate claims:
	
	![zkp-mt1](https://github.com/WildcatsC/eth-projects/blob/main/assets-pics/ZKP3.png)

	![zkp-mt2](https://github.com/WildcatsC/eth-projects/blob/main/assets-pics/ZKP4.png)

	Able to design such a circuit which recreates the calculation model of the root of a merkle tree to verify the received proofs by recalculating the root of the merkle tree.

	[zkp in identity systems](https://github.com/WebOfTrustInfo/rwot8-barcelona/blob/master/topics-and-advance-readings/Zero-knowledge-Proofs.md)
	
   
2. Verkle Update: A better MPT, optimized by reducing witness size.

 - "The main difference between the Verkle tree and the Merkle tree structure is that the Verkle tree is much flatter, meaning there are fewer intermediate nodes linking a leaf to the root, and therefore less data required to generate a proof." [eth org](https://ethereum.org/en/roadmap/verkle-trees/)
	
	[Merkle Tree Path Visualize](https://efficient-merkle-trees.netlify.app)

## Execution Layer: EVM

### 04/07 Skipped

### 04/08 

1. EVM state machine, world state (mapping of all addresses) changing where input is the current state and transactions.
2. Opcode as the lower-level "assembly-like" operations to be readable bytecodes. EIP can propose changes. [EVM data structure animation](https://epf.wiki/#/wiki/EL/evm)
	
 	i. Stack: the good old PUSH and POP
	
 	ii. Program Counter
	
 	iii. **Gas also prevents resource devours by setting computational limitations.**

   		"Since gas restricts computations to a finite number of steps, the EVM is considered quasi Turing complete."	

  	 **Not only an incentive for validators.**
	
 	iv. **Memory** TODO
	
 	v. **Storage** TODO

### 04/09

More on the three important components: gas, memory, storage

[A more detailed illustration of EVM](https://takenobu-hs.github.io/downloads/ethereum_evm_illustrated.pdf)

1. Gas
From the more detailed but clear illustration:
![gas1](https://github.com/WildcatsC/eth-projects/blob/main/assets-pics/Screenshot%202024-04-10%20004707.png)
![gas2](https://github.com/WildcatsC/eth-projects/blob/main/assets-pics/Screenshot%202024-04-10%20004725.png)

3. Memory: read and write using the SLOAD and SSTORE opcode.

   		Memory expansion
 		In EVM, memory is dynamically allocated in multiples of 1 word “pages”. Gas is charged for the number of pages expanded. All locations in memory are well-defined initially as zero.
   
4. Storage: also use SLOAD and SSTORE. Storage is designed as a word-addressed word array. Unlike memory, storage is associated with an Ethereum account and is persisted across transactions as part of the world state.

## Consensus Layer

### 04/10

The p2p network, and 7594, and its relationship with 4844.

The consensus clients participate in a separate peer-to-peer network with a different specification. Consensus clients need to participate in block gossip so that they can receive new blocks from peers and broadcast them when it is their turn to be block proposers. Similar to the execution layer, this first requires a discovery protocol so that a node can find peers and establish secure sessions for exchanging blocks, attestations, etc.

1. EIP-4844
   	Blobs of course. For scalability, support L2 rollups.
    	[danksharding](https://ethereum.org/en/roadmap/danksharding/)
2. EIP-7594 - Peer Data Availability Sampling
	Extend the capabilities introduced by 4844.

	"The intent of a PeerDAS design is to reuse well-known, battle-tested p2p components already in production in Ethereum to bring additional DA scale beyond that of 4844 while keeping the minimum amount of work of honest nodes in the same realm as 4844 (downloading < 1MB per slot)."

 ### 04/11 Skipped

## ETH In the future

 ### 04/12 

[ETH Roadmap Overview](https://ethereum.org/en/roadmap/)
1. The Merge: Transitioned from proof-of-work to proof-of-stake to drastically reduce energy consumption and increase network security​.
2. The Surge: Focuses on scalability enhancements through rollups and data sharding, reducing costs and increasing transaction throughput​.
3. The Scourge: Ensures the network remains secure and decentralized, protecting against various attacks and manipulation​.​
4. The Verge: Introduces Verkle trees to optimize and reduce Ethereum's state, easing processing demands on nodes​.
5. The Purge: Simplifies Ethereum's history and state to reduce storage requirements for node operators​​.
6. The Splurge: Comprises smaller, necessary upgrades to refine the network's functionality and resilience​​.

Impossible triangle of scaling - trilemma

![trilemma](https://github.com/WildcatsC/eth-projects/blob/main/assets-pics/blockchain-trilemma.png)

### 04/13

More on scalability [core changes](https://epf.wiki/?#/wiki/research/scaling/core-changes/core-changes):

The ultimate goals of Ethereum scalability:

 - Transition the execution layer (dApps) entirely to L2 rollups.
   
 - Optimize Ethereum, the L1, to serve primarily as the settlement and data availability layer.

The Consensus Layer (CL) is key for rollups in terms of Data Availability (DA) and for storing proofs of validity, particularly for ZK rollups, as it aims to achieve scalability and reduce Ethereum's gas costs. 

Outline of the development phases for the CL:

1. Proto-danksharding (EIP-4844): This initial phase introduced "blobs" to the network, which are large data batches that help improve scalability by providing a cost-effective way to store large amounts of data necessary for rollups. It was implemented on March 14, 2024.

2. Increasing blob count & gas modifications: Scheduled for the end of 2024, this phase aims to increase the number of blobs that can be included in each block and adjust gas pricing to optimize network throughput and costs.

3. Addition of PeerDAS: This phase involves incorporating a decentralized autonomous system (PeerDAS) for further enhancement of data availability solutions, making the network more robust and scalable.

4. Full implementation of Danksharding: The final phase completes the roadmap by fully implementing Danksharding, which is expected to dramatically increase network capacity and reduce costs by optimizing data shard handling.


### 04/14

Complete walk-through of 4844:

Overview: EIP-4844 introduces "blob-carrying transactions" to scale Ethereum's data availability in a cost-effective manner without fully implementing sharding (still "proto"). It would enable the storage of large data blobs on the beacon chain, further increasing rollup scalability compared to the base chain, by at least two orders of magnitude. 4844 outlines blob transaction parameters, gas accounting, **new opcodes**, and **execution layer changes** to cope with these transactions. This EIP would allow for the shifting to a full sharding paradigm while enabling all the current needs of rollups.

### 04/15 & 04/16 Time Off 

### 04/17 

Learn 4844 thoroughly [here](https://eips.ethereum.org/EIPS/eip-4844)

Before 4844 was live on mainnet, rollups have been storing data only via a section of the transaction known as calldata. 4844  introduces "Blobs", which modifies the current transaction and block structure. 

Aim to: scalability up, cost down. In detail:

1. **Objective:** To temporarily boost Ethereum's scalability by introducing new transaction types that include large data blobs.

2. **Implementation:** Adds blob-carrying transactions, which enable storing large amounts of data intended primarily for use by rollups.

3. **Technical Additions:** New Transactions Types: Specific types that can carry and reference data blobs.

4. **Cryptographic Enhancements:** Methods for secure and efficient handling of large data blobs.

5. **Impact:** Requires modifications at the consensus level to support the new data structures and transaction types.

6. **Scalability Benefits:** Designed to reduce transaction costs for rollups, thereby enhancing overall network efficiency.

7. **Strategic Goals:** Sets foundational changes for Ethereum's transition towards a fully sharded architecture in the future.

### 04/18
Other EIPs regarding data availability.

7624:
1. Objective: Increase the gas cost for calldata to reduce the maximum Ethereum block size.
2. Rationale: Mitigates block size variance and supports the incorporation of data availability blobs as per EIP-4844.
3. New Gas Pricing: Introduces adjustments in gas costs, affecting transactions that use Ethereum mainly for data storage.
4. Block Size Target: Aims to limit the maximum block size to approximately 0.6 MB.
5. Impact on Transactions: Primarily affects data storage transactions; minimal impact on transactions with substantial EVM computation.
6. Implementation: Requires a network upgrade to implement the changes.
7. Overall Goal: Enhance network efficiency and stability without significantly affecting the majority of users.

### 04/19

[About Gas Limit](https://ethresear.ch/t/on-increasing-the-block-gas-limit/18567)
1. By increasing the block gas limit and the price for nonzero calldata bytes, a smaller and less variable block size can be achieved, making space to add more blobs in the future.
2. Increasing the price for nonzero calldata reduces the maximum possible block size. At the same time, the block gas limit could be raised to make more space for regular transactions.
3. This further incentivizes the transition to using blobs for data availability, strengthening the multidimensional fee market by reducing competition between calldata and blobs.
4. It slows down history growth, which might be preferable in preparing for the Verkle upgrades.





