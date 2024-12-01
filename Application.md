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
NTRU(pronounced en-tru¯)真正的模样自然是卷积多项式以及在环上的卷积多项式呀。   

**名词解释**：  
`Z[x]`表示整系数多项式。`(Z/pZ)[x]`表示整系数模p多项式,这个记作 $R_p$,`(Z/pZ)[x]/x^N-1`表示整系数模p多项式模`x^N-1`后的多项式，这个记作 $R_{N,p}$ .  
而多项式模`x^N-1`后就落在了(商)环上。   
中心提升多项式：`(Z/qZ)[x]`上的，系数在 $(-q/2,q/2]$ 之间的多项式。   
乘法逆元多项式： `a(x),a(x)^{-1}`在 $R_p$上， $a(x)a(x)^{-1}\equiv 1\pmod{x^N-1}$   
三元多项式： $T(d_1,d_2)$ 定义为`R`上的多项式，有`d_1`个系数为`1`，有`d_2`个系数为`-1`，其余系数为`0`

**加解密过程**：   
1. 参数`(N,p,q,d)`满足素数`N,p`,`gcd(p,q)=gcd(N,q) = 1`, $q>(6d+1)p$
2. alice选取 $f(x)\in T(d+1,d)$(存在 $R_p,R_q$上的乘法逆元多项式，无则重选)， $g(x)\in T(d,d)$ `f(x),g(x)`作为私钥
3. alice计算`f(x)`在 $R_p,R_q$上的乘法逆元多项式 $F_p(x),F_q(x)$，保存
4. alice计算 $h(x) = F_q(x)\cdot g(x) in R_q$作为公钥
5. 加密：bob将明文转为 $R_p$上的中心提升多项式`m(x)`,选择随机多项式 $r(x)\in T(d,d)$,计算 $e(x)\equiv ph(x)*r(x)+m(x) \pmod{q}$作为密文
6. bob把`e(x)`传给alice，alice计算中心提升多项式 $a(x)\equiv f(x)*e(x)\pmod{q}$
7. 解密：最后alice计算 $b(x)\equiv F_p(x)*a(x)\pmod{p}$ `b(x)`即为`m(x)`

**解密的正确性**：略   

**格的方法**：   
记公钥`h(x)`为 $h(x)=h_0+h_1x+h_2x^2+...+h_{N-1}x^{N-1}$   

the lattice :

$$
M_{h}^{\text{NTRU}} = \left(
\begin{array}{cccc|cccc}
1 & 0 & \cdots & 0 & h_0 & h_1 & \cdots & h_{N-1} \\
0 & 1 & \cdots & 0 & h_{N-1} & h_0 & \cdots & h_{N-2} \\
\vdots & \vdots & \ddots & \vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & h_1 & h_2 & \cdots & h_0 \\
\hline
0 & 0 & \cdots & 0 & q & 0 & \cdots & 0 \\
0 & 0 & \cdots & 0 & 0 & q & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 0 & 0 & 0 & \cdots & q \\
\end{array}
\right)
$$

格的左上为单位矩阵，右上为`h(x)`的系数矩阵，左下为零矩阵，右下为`q`倍单位矩阵。   
通常我们记作：   

$$
\begin{bmatrix} 
1 & h \\
0 & q \\
\end{bmatrix} 
$$

类似与整数的NTRU，令 $f(x)h(x)-qk(x)=g(x)$,有：

$$ \begin{bmatrix} f & -k \\ \end{bmatrix} \cdot 
\begin{bmatrix} 
1 & h \\
0 & q \\
\end{bmatrix} 
= \begin{bmatrix} f & g \\ \end{bmatrix} $$   

**具体情况**：   
具体情况，一般选取规约后范数最小的一行，然后从中提取多项式。   
但提取的多项式不一定是原来的多项式，可能是原来的乘上了同一个多项式模`x^N-1`产生的，但和原来的有一样的效果。

脚本：  
```python
# Sage
# Ref: https://www.osgeo.cn/sagemath/constructions/rings.html
class NTRU:
  def __init__(self, N, p, q, d):
    self.debug = False

    assert q > (6*d+1)*p
    assert is_prime(N)
    assert gcd(N, q) == 1 and gcd(p, q) == 1
    self.N = N
    self.p = p
    self.q = q
    self.d = d
  
    self.R_  = PolynomialRing(ZZ,'x')
    self.Rp_ = PolynomialRing(Zmod(p),'xp')
    self.Rq_ = PolynomialRing(Zmod(q),'xq')
    x = self.R_.gen()
    xp = self.Rp_.gen()
    xq = self.Rq_.gen()
    self.R  = self.R_.quotient(x^N - 1, 'y')
    self.Rp = self.Rp_.quotient(xp^N - 1, 'yp')
    self.Rq = self.Rq_.quotient(xq^N - 1, 'yq')

    # order check in keyGen
    #self.RpOrder = self.p^self.N - self.p
    #self.RqOrder = self.q^self.N - self.q
    self.RpOrder = self.p^(self.N - 1) - 1
    self.RqOrder = (self.q^self.N - self.q) // (self.q-1)


    self.sk, self.pk = self.keyGen()

  def test(self):
    assert self.debug == True
    pass

  def T(self, d1, d2):
    assert self.N >= d1+d2
    t = [1]*d1 + [-1]*d2 + [0]*(self.N-d1-d2)
    shuffle(t)
    return self.R(t)

  # center lift
  def lift(self, fx):
    mod = Integer(fx.base_ring()(-1)) + 1  # emmm
    return self.R([Integer(x)-mod if x > mod//2 else x for x in list(fx)])

  def keyGen(self):
    fx = self.T(self.d+1, self.d)
    gx = self.T(self.d, self.d)

    Fp = self.Rp(list(fx)) ^ (-1)                           # list emmm
    assert pow(self.Rp(list(fx)), self.RpOrder-1) == Fp     # order checked
    assert self.Rp(list(fx)) * Fp == 1                
    
    # Fq = self.Rq(fx) ^ (-1)   # wasted
    Fq = pow(self.Rq(list(fx)), self.RqOrder - 1)     # invert
    assert self.Rq(list(fx)) * Fq == 1                # order checked
    
    hx = Fq * self.Rq(list(gx))

    sk = (fx, gx, Fp, Fq, hx)
    pk = hx
    return sk, pk

  def setKey(self, fx, gx):
    assert type(fx) == type('x^2 + 1')  # e.g.
    assert type(gx) == type('x^2 - 1')  # emmm

    try:
      fx = self.R(fx)
      gx = self.R(gx)

      Fp = self.Rp(list(fx)) ^ (-1)
      Fq = pow(self.Rq(list(fx)), self.RqOrder - 1)
      hx = Fq * self.Rq(list(gx))

      self.sk = (fx, gx, Fp, Fq, hx)
      self.pk = hx
      return True
    except:
      return False
  
  def getKey(self):
    ssk = (
          str(self.R_(list(self.sk[0]))),   # fx
          str(self.R_(list(self.sk[1])))    # gx
        )
    spk = str(self.Rq_(list(self.pk)))      # hx
    return ssk, spk
   
  def encrypt(self, m):
    assert type(m) == type('x^2 + 1') # e.g.
    assert self.pk != None
    hx = self.pk
    mx = self.R(m)
    mx = self.Rp(list(mx))              # change m to Rp, TODO: assert m in Rp
    mx = self.Rq(list(mx))              # change m to Rq

    rx = self.T(self.d, self.d)
    rx = self.Rq(list(rx))
    

    e = self.p * rx * hx + mx
    #return e
    return str(self.Rq_(list(e)))

  def decrypt(self, e):
    assert type(e) == type('xq^2 - 1')  # e.g.
    assert self.sk != None
    fx, gx, Fp, Fq, hx = self.sk

    e = self.Rq(e)
    ax = self.Rq(list(fx)) * e
    a = self.lift(ax)                   # center lift
    bx = Fp * self.Rp(list(a))
    b = self.lift(bx)
    
    #return bx
    return str(self.R_(list(b)))
    
if __name__ == '__main__':
  mm = '-x^2 + x + 1'
  ntru = NTRU(N=11, p=3, q=512, d=3)
  #ntru.setKey('xp^2+1', 'xq^2-1')
  
  print('keyGen check:')
  sk, pk = ntru.getKey()
  print("fx = '%s'" % sk[0])
  print("gx = '%s'" % sk[1])
  print("hx = '%s'" % pk)
  
  print('\nencrypt/decrypt check:')
  e = ntru.encrypt(mm)
  print("e = '%s'" % e)
  m = ntru.decrypt(e)
  print(m)
  assert m == mm
  print(m)

  print('\ncheck setKey:')
  fx = ''
  gx = ''
  hx = ''
  e = ''
  ntru.setKey(fx, gx)
  m = ntru.decrypt(e)
  assert m == mm
  print(m)
```
**未测试**   
代码参考了[Lazzaro](https://lazzzaro.github.io/2020/11/07/crypto-%E6%A0%BC%E5%AF%86%E7%A0%81/#%E6%A0%BC%E5%AF%86%E7%A0%81)   

> 参考了 [HPS](https://link.springer.com/book/10.1007/978-1-4939-1711-2)

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
下面我们认定`n<300`   

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
```python
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
优缺点都很明显：若要与RSA有同等安全性，公钥长度是巨大的。但是即使如此，它因几乎不做模幂运算，而比RSA等高效很多。(LLL算法发表之前)    
若`n<300`，显然不够安全。若`n>300`，公钥长度巨大。(LLL算法发表之后)   

> 参考了 [HPS](https://link.springer.com/book/10.1007/978-1-4939-1711-2)

## GGH    
格密码中诞生比较早的加密方案。并不安全。   

**加解密过程** :  
1. alice选择一组好基`V`作为私钥，并乘上矩阵使之变为坏基，作为公钥`W` $W=[...,w_i,...,w_n]$
2. 加密：alice把公钥传给bob，bob将消息转为小向量（比如二进制）`m` $m=(...,m_i,...,m_n)$
   ，随机选取小向量`r`，计算新向量`c`, $c=mW+r=\sum_{i=1}^{n}m_iw_i +r$ 该向量作为密文
3. 解密：bob把密文`c`传给alice，alice用babai算法得到`c`的最近向量`e`，然后计算 $eW^{-1}$ 得到明文   

毕竟有了LLL算法，也没多少安全性了。











## LWE   
## HNP   
## others    
### 1   
一个很好的例子。   
设n=pq,然后构造了一个矩阵p $ q // ? $ ? // 问号是比p,q比特位数长好几倍的大数。   
然后造了一个随机2*2矩阵，和前面的p,q矩阵相乘，输出结果。   
很明显的，对结果矩阵用LLL()就能求出p,q来。   
（待续
