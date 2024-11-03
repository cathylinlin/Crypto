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







