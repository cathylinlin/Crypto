# Application of Lattice
## NTRU    
### -----the simplest case------   
**加解密过程**：   
1. 取大（素）数 `p`,`g`,随机数 `f`,   
    $g<f<p$  
    $f<\sqrt{p/2},,\sqrt{p/4}<g<\sqrt{p/2},,gcd(f,pg)=1$  
2. 取 $h\equiv g\cdot f^{-1}\pmod p$  
3. 取随机数 `r`,message `m`,   
    $0<r<\sqrt{p/2}$   
    $0<m<\sqrt{p/4}$  
    这些限制条件可以推出： $rg+mf<p$   
4. 加密:  ciphertext `c`  $c\equiv rh+m \pmod p$  
5. 解密: 取 $a\equiv cf\pmod p,m\equiv a\cdot f^{-1}\pmod g$   

**解密的正确性**：   
$a\equiv cf\equiv frh+fm\equiv frgf^{-1}+fm\equiv rg+fm\pmod p$   
$rg+mf<p\Rightarrow a=rg+fm$   
$m\equiv (a\pmod g)\cdot f^{-1}\pmod g$   

**格的方法**    
经常，我们只有`g`和`h`。但我们可以找到一个`F`和一个`K`，它满足：  
$Fg\equiv h\pmod p \Rightarrow Fg=h+Kp$  
$F(1,h)-K(0,p)=(F,g)$  
那这个`F`和`K`的功能和加密者用的`f,k`是一样的，这也近似完成了解密。   

具体的：
$h\equiv g\cdot f^{-1}\pmod p$   
$hf\equiv g\pmod p$   
$fh-kp=g$   
(f=f)   
the lattice:  

$$ \begin{bmatrix} f & -k \\ \end{bmatrix} \cdot 
\begin{bmatrix} 
1 & h \\
0 & p \\
\end{bmatrix} 
= \begin{bmatrix} f & g \\ \end{bmatrix} $$   

but,why this?And how to solve?   

首先`1,0,h,p`不是好的格基，其中`h`和`p`是几乎一样大的。接着我们知道这个格可以表示出含`f,g`（我们要求的就是f,g）的一组基，而且根据加密过程,`f,g`是远小于`h,p`的。这就近似于SVP问题。   
而LLL算法刚好是一个可以近似解决SVP问题的好算法。那自然可以借此大概率求出`f,g`来。  
也就完成了解密。   

**更难的情况**：  
稍难一点的题目则会出现`f,g`并非最短向量的情况，那我们就需要在格和基都配上一个 `D（=2**？）`，使格的行列式大于含`f,g`的基的范数。这也就是“配平”。    
再难一点的题目会让你构造3*3的格，只要记住我们在格的最后一列处理关系式，其他两列用恒等式（以及配平）即可。尽量不要让基的数差距过大，比如`[a,1,b]`.    

### -----its true form-----    
NTRU真正的模样自然是卷积多项式以及在环上的卷积多项式呀。   
`Z[x]`表示整系数多项式。`(Z/pZ)[x]`表示整系数模p多项式 `(Z/pZ)[x]/x^N-1`表示整系数模p多项式模`x^N-1`后的多项式，这个记作 $R_{N,p}$ .  
而多项式模`x^N-1`后就落在了(商)环上。   
NTRU的加解密过程更新了多次，我尽量将重要的讲解出来。   
（待续   

## knapsack(pack)    
背包问题的内核是一个NP完全问题——子集和问题。  

设 $M=set(M_1,M_2,...,M_n)$ 里面的元素是递增的。这个集合作为公钥。   
Alice将她的消息转化为二进制向量 $x=(x_1,x_2,...,x_n)$ （ $x_i =0 or 1$）（可以通过调试让dim(x)和card(M)相等）  
然后Alice计算 $S=\sum_{i=1}^{n}x_iM_i$ 值`S`作为密文传给bob   

Bob想要解密，最简单的碰撞方法就是中间相遇攻击。但这在`n`非常大时几乎失效。   

因此密码学家将集设定为超递增的，即满足里面任意元素大于前一元素的二倍。这可以推出里面任意元素大于之前全部元素之和。   
$x_i>2x_{i-1} \Rightarrow x_n>\sum_{i=1}^{n-1}x_i$   

**加解密过程**：   
1. 取超递增集 $r=set(r_1,r_2,...,r_n)$
2. 取大数`A,B`， $B>2r_n且gcd(A,B)=1$
3. 取 $M=set(...,M_i,...)$ 作为公钥  
   $M_i\equiv Ar_i\pmod B$
4. 加密： message`m`转为二进制向量 $x=(x_1,x_2,...,x_n)$
   计算 $S=\sum_{i=1}^{n}x_iM_i$ 值`S`作为密文
5. 解密： $S'\equiv A^{-1}S\pmod{B}$ 然后`S'`与`r`里元素做差比较求出二进制向量，进而得到`m`  

**格的方法**：   
1985年，LLL算法论文发表后，背包加密的致命弱点显露了出来。   

公钥： $M=set(M_1,M_2,...,M_n)$    
密文： $S=\sum_{i=1}^{n}x_iM_i$   
解： $x=(x_1,x_2,...,x_n)$   

我们取横向量   
$v_1    =(2,0,0,...,0,M_1)$   
$v_2    =(0,2,0,...,0,M_2)$   
$v_n    =(0,0,0,...,2,M_n)$   
$v_{n+1}=(1,1,1,...,1,S)$   

the lattice :  

$$
\begin{bmatrix} 
2 & 0 & 0 & \cdots & 0 & M_1 \\
0 & 2 & 0 & \cdots & 0 & M_2 \\
0 & 0 & 2 & \cdots & 0 & M_3 \\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & 0 & \cdots & 2 & M_n \\
1 & 1 & 1 & \cdots & 1 & S \\
\end{bmatrix} 
$$

令  $t=\sum_{i=1}^{n}x_iv_i -v_{n+1}$  
这是一个坐标只有 $\pm 1$ 的向量。模长约是 $\sqrt{n}$   
应该感到惊奇，格的每个向量模长都是非常大(2**2n)的，但它却能表示出如此小的向量。`t`几乎可以说是最小向量了。(SVP问题)   
那就用LLL！  
然后就很大概率解决了这个加密。   

脚本：   
```
from Crypto.Util.number import *
from gmpy2 import *
from tqdm import tqdm

pubkey=[]   #公钥
c=     #密文

#造格
n=len(pubkey)
L = matrix(ZZ,n + 1,n + 1)
for i in range(n):
    L[i,i] = 2
    L[i,n] = pubkey[i]

for i in range(n):
    L[len(pubkey),i]=1
    
L[-1,-1] = c

# LLL
L = L.LLL()
#print(L)

for i in tqdm(range(n)):
    if(L[i][-1]!=0):
        continue
    for j in range(n):
        if((L[i][j])**2!=1):
            continue
        else:
            Lm=L[i]

Lm=Lm*-1   #看情况是否需要*-1
#print(Lm)
mm=''
for i in range(n):
    mm+=str((Lm[i]+1)//2)
print(long_to_bytes(int(mm,2)))
```
**测试完成，已修正**

代码参考了 [Emmmaaaaaaaaaa](https://blog.csdn.net/XiongSiqi_blog/article/details/132109655)

**评价**：   
优缺点都很明显：若要与RSA有同等安全性，公钥长度是巨大的。但是即使如此，它因几乎不做模幂运算，而比RSA等高效很多。   
（待续   



> 参考了 [HPS](https://link.springer.com/book/10.1007/978-1-4939-1711-2)



## LWE   
## HNP   
## others    
### 1   
一个很好的例子。   
设n=pq,然后构造了一个矩阵p $ q // ? $ ? // 问号是比p,q比特位数长好几倍的大数。   
然后造了一个随机2*2矩阵，和前面的p,q矩阵相乘，输出结果。   
很明显的，对结果矩阵用LLL()就能求出p,q来。   
（待续
