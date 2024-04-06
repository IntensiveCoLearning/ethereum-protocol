# loxia

哈喽大家，我是 loxia.eth，来这里补课学习一些基础且重要的知识，很高兴能来到这里和大家一起学习。

目录：
4.5 ECDSA
4.6 ECDSA。

## Notes

### 2024.4.7



### 2024.4.6

Day2：今日完成 https://epf.wiki/#/wiki/Cryptography/ecdsa 的学习

Elliptic curves over finite field 有限域上的椭圆曲线
有限域椭圆曲线加法群有良好的循环特性，适合作为密钥生成的场景。
例：Alice 利用如下参数生成了一条曲线
sage: E = EllipticCurve(GF(997),[0,7])
Elliptic Curve defined by y^2 = x^3 + 7 over Finite Field of size 997

在曲线上随机生成点 G，标量相乘 n 次后到达无穷远点 O，n(G) = O
Alice 从 n 中随机选择了 42 作为私钥 K，公钥则为 P 点，P = 42(G) 也就是 P = K*G
sage: P = K*G
(858 : 832 : 1)   P 公钥则为 (858,832)

Alice 发送交易信息时会先将交易信息取哈希值 m，对于每个签名都有临时密钥对的生成减轻暴露的攻击
Randomly selected ephemeral secret key.
sage: eK = 10
Ephemeral public key.
sage: eP = eK*G
(215 : 295 : 1)

Ephemeral key pair =[eK,eP]=[10,(215,295)].
计算签名的组件 s = (k^-1)(e+rK)(modn)
其中 r 是 eP 的 x 轴值，这个签名中同时用到了私钥 K 和 临时密钥对

# x-coordinate of the ephemeral public key.
sage: r = int(eP[0])
215
# Signature component, s.
sage: s = mod(eK**-1 * (m + r*K), n)
160

签名对是 (r,s)=(215,160) 

Bob在拿到签名对时展开验证，首先验证交易哈希，其次计算临时公钥 R，并以此确认 r。
sage: R = int(mod(m*s^-1,n)) * G  + int(mod(r*s^-1,n)) * P
(215 : 295 : 1)
Compare the x-coordinate of the ephemeral public key.
sage: R[0] == r
True # Signature is valid ✅

哈希变化或者私钥错误，交易都会验证失败，ETH 中的实际情况要比这复杂，详见
https://web.archive.org/web/20240229045603/https://lsongnotes.wordpress.com/2018/01/14/signing-an-ethereum-transaction-the-hard-way/




### 2024.4.5

Start here！从 epf.wiki 的底部目录先开始看吧，我一直没有看过椭圆曲线签名的细节，今儿个来学学数学~~~

ECDSA: Elliptic Curve Digital Signature Algorithm
ECC: Elliptic curve cryptography
交易中需要验证交易中的真实授权，以及信息未被篡改。
以太坊使用的椭圆曲线标准为 secp256k1：http://www.secg.org/sec2-v2.pdf   其中a=0，b=7，y^2 = x^3 + 7

搜到 Secp256k1 作为基于Fp有限域上的椭圆曲线，由于其特殊构造的特殊性，其优化后的实现比其他曲线性能上可以特高30％，有明显以下两个优点：
1）占用很少的带宽和存储资源，密钥的长度很短。
2）让所有的用户都可以使用同样的操作完成域运算。

But Why？不过先放下性能问题，我先搞明白原理

Group：群，Field：域。椭圆曲线有趣在于曲线上两个点形成一个组，两点“相加”的结果保留在曲线上。这是一种几何加法，通过绘制选择的两个点为一条直线并在 x 轴上取曲线交点为和。
椭圆曲线加法群：定义为椭圆曲线在实数域上的点集以及点加法。

当两点重复时，所绘制的为 P 点切线，在这种情况下，直线将成为反映总和 (2P) 的 P 的切线。
Repeated point-addition is known as scalar multiplication:重复点加法称为标量乘法
nP = P + P + P + P（加 n 次）


Discrete logarithm problem 离散对数问题
标量乘法可以用作生成密钥
私钥表示为 K，代表基点 G 自相加的次数，最终产生公共点 P
P = K * G
我们要确保标量乘法不会泄露我们的私钥，这要确保标量乘法“容易”，且“不可追踪”。
模运算引入环绕效应，对于素数模，这是特别困难的，被称为离散对数问题。
