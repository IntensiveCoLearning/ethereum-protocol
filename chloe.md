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

