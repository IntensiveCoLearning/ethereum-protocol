# breeze
I'm breeze, a Product Engineer specialized in JavaScript, Electron and automation scripts. Recently, I'm exploring web3. Excited to learn and contribute here!


## Notes

### 2024.4.6

todo：
刷完视频：
- https://www.youtube.com/watch?v=bBC-nXj3Ng4
- 继续week0计划


### 2024.4.5
1. 了解了epf.wiki的计划，目前看下来不涉及到具体的开发，更多的是针对Eth底层协议的学习；
2. 对称加密和非对称加密过程和原理；以及可以关注一下[Signal](https://github.com/signalapp)的开源实现
3. Merkle trees 构建步骤：
 -  数据分块 首先,将需要验证的大量数据分割成等长的小块(例如512字节或1KB),并计算每个小块的哈希值。这层哈希值被称为"叶节点(Leaf Nodes)"。
- 构建中间层节点 将相邻的两个叶节点哈希值进行拼接(或直接连接),再对拼接后的值进行哈希运算。得到的新哈希值称为"父节点"。 例如,如果叶节点分别是H(0)和H(1)(H表示哈希函数),则父节点为H(H(0)||H(1))。
- 迭代哈希运算 重复第2步的过程,将相邻的父节点哈希值拼接并哈希,形成新的父节点,直至剩下一个根节点。这个根节点就是整个数据的"数字指纹"或"摘要"。
- 构建完整的Merkle树 最终,Merkle树是一个完整的二叉树,根节点表示整个数据集的摘要,叶节点表示原始数据块的哈希值,中间层节点表示相应数据块的组合摘要。

如图所示：
    ![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Hash_Tree.svg/2560px-Hash_Tree.svg.png)

使用这个数据结构的优点：
- 高效验证数据完整性:只需提供相应的部分叶节点哈希值和对应节点的路径,就可以重新计算出根节点哈希值,并与已知的根哈希值进行比对,从而验证数据的完整性。
- 节省存储空间:只需存储根哈希值和一些关键节点,而不需存储整个树,即可验证庞大数据集。
- 支持高效数据修改:修改部分数据只需重新计算涉及的节点,而不影响整个树的其他部分。

todo： 目前只是大概知道了节点里面的计算过程，但是整体在eth里面block是如何创建的还需要了解清楚；