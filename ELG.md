# ELG  
## DLP  
## DH  
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







