# Attack from p,q   
一些针对 $p$ , $q$ 的攻击。   

## many $p$   
多素数 $n$ ，实际上是考察会不会算欧拉函数。   

## $e\mid phi$   




## $e\mid p$    
AMM算法。   




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
```
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




