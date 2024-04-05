# loxia

哈喽大家，我是 loxia.eth，来这里补课学习一些基础且重要的知识，很高兴能来到这里和大家一起学习。

## Notes

### 2024.4.6

Elliptic curves over finite field 有限域上的椭圆曲线：

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
