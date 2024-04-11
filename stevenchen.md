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

 








