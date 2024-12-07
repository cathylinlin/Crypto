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

解密的正确性：略   

Elgamal和DH密钥交换是一个难度的。如果一个人可以解决随机生成的Elgamal,那么他也可以解决DH密钥交换。  
证明：略  

目前仍无快速解决的方法，只能依靠加密者选取了特殊构造的数字来针对性解密。  
常见方法有：小步大步，Pollard-rho.

## DSA   





