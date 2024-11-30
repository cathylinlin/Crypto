# Attack from p,q   
一些针对 $p$ , $q$ 的攻击。   

## many $p$   
多素数 $n$ ，实际上是考察会不会算欧拉函数。   

## $e\mid \varphi$    
$e\mid \varphi$ ，可以检验一下 $e\mid p-1,q-1?$ 如果有一个不，就以那个作为模数做常规rsa。    
但这是有条件的，这适用于 $m$ 的比特位数小于 $p-1,q-1$    



## $e\mid p-1$    
### AMM算法。   
### 有限域开方 + CRT   
适用于 $e$ 比较小，且 $e\mid p-1,q-1$ 的情况。   
```python
R.<x> = Zmod(p)[] #还有q
f = x ^ e - c
f = f.monic()
res1 = f.roots()
prnt(res1)

from Crypto.Util.number import  *
import libnum
#p,q,n,c,e
# res1,res2
for i in res1:
    for j in res2:
        m =libnum.solve_crt([int(i[0]),int(j[0])],[p,q])     #c3=libnum.solve_crt([c1,c2], [q1,q2])
        flag = long_to_bytes(m)
        if flag.startswith(b'flag'):
            print(flag)
```
代码参考了[Emmaaaaaaaaaa](https://blog.csdn.net/XiongSiqi_blog/article/details/130296035)    


## ${q}^{-1}\cdot q\equiv 1\pmod{p}$     
题目给了 ${q}^{-1}$ $d$ $c$ $e$ ，且 $d$ 的比特位数和 $n$ 相等。   
根据 $ed=k\varphi +1$ ，可在小于 $e$ 的范围内爆破出 $\varphi$     
然后我们任意去两数 $g_1 g_2$ 满足 $gcd(g,p)=1$    
（不过一般 $p$ 就是素数，取个3和5就差不多了）   
${g}^{\varphi}\equiv {g}^{(p-1)(q-1)}\equiv 1\pmod{p}$    
这里我们用到了费马小定理   
于是可得 ${g_i}^{\varphi}-1=k_ip$  用 $gcd(g_1,g_2)$  就可以求出p了    
(当然要保证一下比特位数)

不过容易注意到 ${q}^{-1}\cdot \varphi\equiv {q}^{-1} -1\pmod{p}$     

## p leak     
p高位泄露。    
常见于p>>t    
暂记为 ${p}_{high}=p_0$ ，泄露位数为 $h$ ， $p$ 位数为 $t$ ，未知部分为 $x$   
$p=p_0\cdot {2}^{t-h}+x$    
为解出 $x$ ，我们构造 $PR.< x >$ ，并求出small_roots    
```python
PR.<x> = PolynomialRing(Zmod(n))
f = p0*2**(h-t)+x
f = f.monic()
out_p = f.small_roots(2**?,beta = 0.4,epsilon = 0.03)
p = gcd(int(f(out_p[0])),n)
print(p)
```
p低位泄露，中间位泄露同理。    
值得注意的是，上述代码适用于未知位数/总位数<=0.44的情况。    

--------------------------------------------------      
倘若给了p高位，q低位，而且都比较少，又怎么弄呢？   
设q_low位长x，则有：   
${q}_ {low}\equiv q\pmod{2^x}$    
$n=pq=p(k2^x+{q}_ {low})$   
$n\equiv p{q}_ {low}\equiv {p}_ {low}{q}_ {low}\equiv {n}_{low}\pmod{2^x}$     
于是就转变为p的首尾泄露了。    


## p^q(xor)   
不知道怎么说，反正就是一位一位爆破。    
```python
# 爆破p,q，从低位开始
plist,qlist = [0],[0]
mod = 1
for i in range(1024):
    mod *= 2
    next_plow,next_qlow = [],[]
    for pl,ql in zip(plist,qlist):
        for ph,qh in product([0,1],repeat = 2): # 得到(0,0),(0,1),(1,0),(1,1)
            p_mod = ph * (mod // 2) + pl
            q_mod = qh * (mod // 2) + ql
            if p_mod * q_mod % mod == n % mod and  p_mod ^ q_mod == p_xor_q % mod:
                next_qlow.append(q_mod)
                next_plow.append(p_mod)
    plist,qlist = next_plow,next_qlow
```
代码来自 [Emmmaaaaaaaaaa](https://blog.csdn.net/XiongSiqi_blog/article/details/131025906)    




