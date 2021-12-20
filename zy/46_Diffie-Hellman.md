# Diffie-Hellman 算法

&emsp;&emsp;Diffie-Hellman 是非对称算法，主要用途是进行密钥交换，不用作加密、解密信息。它的安全性源于在有限域上计算离散对数比计算指数更为困难。举例，如果甲、乙两个人需要交换密钥，计算机语言描述如下：

1. 甲构建密钥对：Private_Key_1 和 Public_Key_1
2. 甲向乙公布 Public_Key_1
3. 乙基于甲的 Public_Key_1 构建密钥对：Private_Key_2 和 Public_Key_2
4. 乙向甲公布 Public_Key_2
5. 甲使用自己的 Private_Key_1 和 乙的 Public_Key_2 构建本地密钥
6. 乙使用自己的 Private_Key_2 和 甲的 Public_Key_1 构建本地密钥
7. 甲和乙互相通信时，都使用上面构建的本地密钥对数据进行加密和解密
8. 虽然甲、乙使用不同的密钥构建自己的本地密钥，但是双方获得的本地密钥相同。（此为关键要点）

&emsp;&emsp;数学上的解释较为复杂，需要先理解离散对数的概念。定义素数 p 的原根（Primitive Root）为 a，对于任意数 b，可以找到唯一的指数 i，满足 b = a^i mod p，其中 0 <= i <= p-1，那么 i 被称为 b 的以 a 为基数的模 p 的离散对数。Diffie-Hellman 算法的可靠度建立在：已知大素数 p 和 原根 a，对于给定的 b 计算出 i 极其困难，而给定 i 计算出 b 却相对容易。用数学语言描述如下：

1. 甲和乙事先约好大素数 p 和原根 a
2. 甲随机产生一个数 x，计算 X = a^x mod p，然后把 X 发送给乙
3. 乙随机产生一个数 y，计算 Y = a^y mod p，然后把 Y 发送给甲
4. 甲计算 k = Y^x mod p
5. 乙计算 k' = X^y mod p
6. 根据数学运算可推导出 k = k'，它们就是甲和乙独立计算出的本地密钥

&emsp;&emsp;在不安全的网络中，窃听者可以获得 p、a、X、Y，却无法计算出 k 或者 k'，因为计算离散对数（即 x 和 y）十分困难。第六步的数学运算为：k = Y^x mod p = (a^y)^x mod p = (a^x)^y mod p = X^y mod p = k'，得证。

&emsp;&emsp;原根的定义：设 p 为正整数，a 为整数，若 a mod p 的阶等于 φ(p)，则称 a 为 p 的一个原根。φ(p) 表示为 p 的欧拉函数，含义是小于或等于 p 的正整数中与 p 互质的正整数的数目。例如 φ(8) = 4，因为 1、3、5、7 与 8 互质，个数为 4。假设一个数 a 是 p 的原根，那么 a^i mod p 的结果两两不同余（a 在 1 和 p 之间；i 在 0 和 p 之间），也就是说上例的 X ≠ Y。

&emsp;&emsp;维基百科对原根的定义不涉及「阶」，比较通俗易懂。对于两个互质的正整数 gcd(a,m) = 1，由欧拉定理可知，存在正整数 d <= m，比如说 d = φ(m)，即小于等于 m 的正整数中与 m 互质的正整数的个数，使得 a^d ≡ 1 (mod m)。因此，在 gcd(a,m) = 1 时，定义 a 对模 m 的指数 δ<sub>m</sub>(a) 为使 a^d ≡ 1 (mod m) 成立的最小正整数 d。由前知，δ<sub>m</sub>(a) 一定小于等于 φ(m)，若 δ<sub>m</sub>(a) = φ(m)，则称 a 是模 m 的原根。

&emsp;&emsp;※ 艾妙妙，二〇一九年十二月十六日 ※