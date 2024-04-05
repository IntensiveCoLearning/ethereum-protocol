# Derick

hi guys, my name is Derick and I'm a back-end programmer who loves technology. I'm looking forward to learning about the Ethereum Protocol by attending https://epf.wiki/

## Notes

### 2024.4.5

#### week0中的Cryptography
- 散列 (Hashing)详解
散列函数像是信息摘要工具，可以将任意长度的输入数据转换成固定长度的输出结果，称为散列值 (Hash 值)。这个过程就像把一堆文件的内容浓缩成一个简短的指纹一样。散列函数有以下关键特性：

1. 易于计算 (Easy to Compute)： 任何人都能快速计算出给定数据的散列值。
2. 抗碰撞 (Collision Resistant)： 对于不同的输入数据，产生相同散列值的可能性极低。就好比不同的人几乎不可能有相同的指纹一样。
3. 单向性 (One-Way)： 仅凭散列值本身，几乎无法推导出原始的输入数据。就如同指纹只能用来验证身份，却无法还原成完整的个人信息。
- 散列函数在密码学领域有着广泛的应用：

- 数字签名 (Digital Signature)： 利用散列函数对数据进行签名，可以确保数据的完整性 and 可靠性 (可靠性 - refers to authenticity in this context)。就好比给重要文件盖上印章，可以验证发送者身份并且确保内容没有被篡改。
- 数据校验 (Data Integrity Check)： 通过比较原始数据的散列值和传输过程中的散列值，可以判断数据是否在传输过程中被修改。就如同核对货物出库时的封条一样，可以确保货物在运输途中没有被掉包。
- 密码存储 (Password Storage)： 网站不会存储用户的原始密码，而是存储密码的散列值。这样即使黑客窃取了数据库，也无法获得用户的明文密码。就好比指纹识别系统只存储指纹的特徵，而不是整只手的外貌。
常用的散列函数包括 MD5、SHA-1 和 SHA-2 家族。

- 公钥密码学 (Public Key Cryptography)详解
公钥密码学采用密钥对 (Key Pair) 的方式进行加密和解密。密钥对包含公钥 (Public Key) 和私钥 (Private Key)。形象地比喻，公钥就像是邮箱的地址，所有人都可以知道；而私钥就像邮箱的钥匙，只有持有者才能打开邮箱。

- 公钥密码学的工作流程如下：

加密 (Encryption)： 信息发送者使用接收方的公钥对信息进行加密。 任何人都可以用公钥加密信息，但是只有持有私钥的人才能解密。
解密 (Decryption)： 信息接收者使用自己的私钥解密密文，还原成原始信息。 因为只有持有私钥的人才能解密，所以可以确保信息的保密性。

常用的公钥密码算法包括 RSA 算法

#### Merkle 树详解
- Merkle 树是一种树形数据结构，常用于高效验证大型数据的完整性。它利用密码学中的散列函数来构建，具有以下特点：

1. 数据完整性校验 (Data Integrity Verification)： 可以方便地校验数据块 (Data Block) 的完整性，而无需检查整个数据。
2. 去重存储 (Deduplication)： 可以识别并消除数据中的重复部分，节省存储空间。
##### Merkle 树的结构
Merkle 树由节点 (Node) 组成，每个节点包含以下信息：

- 数据块哈希 (Hash of Data Block)： 对数据块计算所得的散列值。
- 子节点哈希 (Hashes of Children Nodes)（可选）： 对于非叶子节点，包含其子节点的哈希值。
Merkle 树的层级结构清晰，叶子节点代表原始数据块的哈希值，非叶子节点的哈希值由其子节点的哈希值计算得到。计算方法通常采用一种称为 "Merkle-Damgård Hash Function" 的技术，例如 SHA-256。

##### Merkle 树的工作原理

1. 在Merkle树中,每个叶子节点都标记了一个数据块的加密哈希值,而每个非叶子节点则标记了其子节点标签的加密哈希值。

2. 叶子节点的数据先被哈希,然后成对分组。每对节点的哈希值再被哈希,生成它们的父节点。重复这个过程,直到只剩下一个根节点,称为Merkle根。

3. Merkle树通常使用二叉树结构,即每个节点最多有2个子节点。但也可以使用多叉树,每个节点有多个子节点。

4. 为了验证某个叶子节点是否属于Merkle树,只需计算沿着该叶子到根的路径上的哈希值,数量与树的深度成正比。这比在哈希列表中逐个检查每个节点要高效得多。

5. 如果Merkle树的任何部分发生变化,都会传播到根节点。通过比较根哈希,可以快速检测出数据是否被篡改。

6. Merkle树的搜索、插入、删除操作复杂度为O(logn),空间复杂度为O(n),其中n是叶子节点数。

Merkle树利用哈希指针构建一个树形结构,可以高效地验证大型数据结构的完整性,广泛用于区块链、分布式系统、版本控制等领域中。它通过递归哈希的方式,在保证安全性的同时提供了更好的性能。

[1] https://www.youtube.com/watch?v=3AcQyTs_Es4&t=0

[2] https://brilliant.org/wiki/merkle-tree/

[3] https://www.geeksforgeeks.org/blockchain-merkle-trees/

[4] https://en.wikipedia.org/wiki/Merkle_tree

### 2024.4.3
