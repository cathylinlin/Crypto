# ELG  
## DLP   
The Discrete Logarithm Problem   
离散对数问题  

对于一个有限域`Fp`（p为素数），总存在一个原根`g`，使得域内非零元素都是`g`的幂。  
离散对数问题：给定`Fp`和它的原根`g`，对于域内任意一个非零元素`h`，求出`x`使之满足 $g^X\equiv h\pmod p$   

容易知道，解是无限的，根据费马小定理， $g^{p-1}\equiv 1\pmod p$，如果求出一解`x0`，那 $x=x_0+k(p-1),k\in Z$都是该问题的解。   
也就是说，求离散对数给出了一个从`Fp*`到`Z/(p-1)Z`的映射（群同构）    

目前仍无效率较高的算法，只能依靠`p`是有特殊构造来针对处理。  
比如当`p-1`易分解时，可以使用The Pohlig–Hellman Algorithm.   

## DH   
The Diffie–Hellman key exchange algorithm   
DH密钥交换算法   

过程：  
1. alice和bob选择同一个大素数`p`，选择同一个原根`g`作为公开参数
2. alice和Bob各秘密选择数`a`(`b`)，计算 $A\equiv g^a\pmod p$   
   $B\equiv g^b\pmod p$  然后互相交换结果
3. 二人得到对方结果后再计算 $A'\equiv B^a\pmod p$  
   $B'\equiv A^b\pmod p$  然后进行后续加密   

容易知道`A'=B'`，证明略。  

DH比DLP简单一点。如果一个人能解决DH，我们并不确定他能否解决DLP。   

## Elgamal  
The Elgamal Public Key Cryptosystem   
Elgamal是一种公钥密码系统。其中的困难问题是求离散对数。   

加解密过程：
1. 二人选择一个大素数`p`作为公钥之一，然后选择阶数较大的生成元`g`作为公钥之一
2. Alice选择小于`p-1`的数`a`作为私钥，计算 $A\equiv g^a\pmod p$作为公钥
3. bob将明文`m`处理到小于`p`，然后选取一随机数`k`，计算 $c_1\equiv g^k\pmod p$  
   $c_2\equiv mA^k\pmod p$,然后他把(c1,c2)传给Alice
4. Alice计算 $m\equiv c_2(c_1^a)^{-1}\pmod p$   

解密的正确性：略（尝试拆开来再合并）   

Elgamal和DH密钥交换是一个难度的。如果一个人可以解决随机生成的Elgamal,那么他也可以解决DH密钥交换。  
证明：略（突破点：`g^ab`）  

目前仍无快速解决的方法，只能依靠加密者选取了特殊构造的数字来针对性解密。  
常见方法有：小步大步，Pollard-rho.   

## DSA   
### RSA  
和RSA加解密过程几乎一模一样啊。  

过程：    
1. S选择大质数`p,q`，然后计算`n=pq`，选择签名加密指数`e`，公开`(N,e)`
2. S计算`e`关于 $\varphi(N)$的模逆元`d`，对需要传输的信息`D`签名 $S\equiv D^d\pmod n$,并传给V，这一步就是签名
3. V计算 $D'\equiv S^e\pmod n$,并确认`D'`和`D`是否相同，这一步就是验证。

### Elgamal    
典中典`2 to 1`

过程：   
1. 选择大素数`p`，选择一原根`g`，公开`(g,p)`
2. S选择小于`p`的随机数`a`，计算 $A\equiv g^a\pmod p$,公开认证钥匙`A`
3. S将信息`D`处理到小于`p`，选择小于`p`，且和`p-1`互素的随机数`k`  
   计算 $S_1\equiv g^k\pmod p$ 和 $S_2\equiv (D-aS_1)k^{-1}\pmod{p-1}$   
   将`(S1,S2)`传输过去，这一步就是签名
4. V接收到后，验证 $A^{S_1}S_1^{S_2}\equiv g^D\pmod p$，这一步就是验证。

正确性：略（尝试拆开来再合并）   















