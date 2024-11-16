# Attack From Many Times      
一些针对很多 $n$ 或 $m$ 的攻击。    


## many $e$     
### 共模攻击    
已知 $c_1\equiv m^{e_1}\pmod{n}$ and $c_2\equiv m^{e_2}\pmod{n}$  并且 $gcd(e_1,e_2)=1$    
求出 $s_1,s_2$ ,使得 $e_1s_1+e_2s_2=1$    
$m\equiv {c_1}^{s_1}\cdot {c_2}^{s_2} \pmod{n}$     
显然，把指数提出来再代入就有了    
```
s0,s1,s2 = gmpy2.gcdext(e1,e2)
print(s0)
m_s0 = pow(c1,s1,n)*pow(c2,s2,n)%n
m = long_to_bytes(gmpy2.iroot(m_s0,s0)[0])
print(m)
```
参考了[这位师傅](https://blog.csdn.net/XiongSiqi_blog/article/details/130836354)      


## many $m$    
### Relater Message Attack    
形如 $c_1\equiv m^e\pmod n$   
$c_2\equiv (am+b)^e\pmod n$    
将此二式化为模 $n$ 意义下的多项式，$x-m$ 显然是这两多项式共同的因式。用欧几里得算法处理。   
```
import binascii
def franklinReiter(n,e,c1,c2,a,b):
    PR.<x> = PolynomialRing(Zmod(n))
    g1 = (x)^e - c1
    g2 = (a*x+b)^e - c2

    def gcd(g1, g2):
        while g2:
            g1, g2 = g2, g1 % g2
        return g1.monic() # 
    return -gcd(g1, g2)[0]

m=franklinReiter(n,e,c1,c2,a,b)
print(libnum.n2s(int(m)))
```
代码参考了[这位师傅](https://blog.csdn.net/XiongSiqi_blog/article/details/130978226)   
值得一提的是，上述脚本适合加密指数较小的情况。     

### short-pad attack   
上述的b 未知，但是比较小。   
原理，暂不了解。
```
#Sage
def short_pad_attack(c1, c2, e, n):
    PRxy.<x,y> = PolynomialRing(Zmod(n))
    PRx.<xn> = PolynomialRing(Zmod(n))
    PRZZ.<xz,yz> = PolynomialRing(Zmod(n))
    g1 = x^e - c1
    g2 = (x+y)^e - c2
    q1 = g1.change_ring(PRZZ)
    q2 = g2.change_ring(PRZZ)
    h = q2.resultant(q1)
    h = h.univariate_polynomial()
    h = h.change_ring(PRx).subs(y=xn)
    h = h.monic()
    kbits = n.nbits()//(2*e*e)
    diff = h.small_roots(X=2^kbits, beta=0.4)[0]  # find root < 2^kbits with factor >= n^0.4
    return diff
def related_message_attack(c1, c2, diff, e, n):
    PRx.<x> = PolynomialRing(Zmod(n))
    g1 = x^e - c1
    g2 = (x+diff)^e - c2
    def gcd(g1, g2):
        while g2:
            g1, g2 = g2, g1 % g2
        return g1.monic()
    return -gcd(g1, g2)[0]
if __name__ == '__main__':
    n = 
    e = 
    c1 =
    c2 = 
    diff = short_pad_attack(c1, c2, e, n)
    print("difference of two messages is %d" % diff)
    m1 = related_message_attack(c1, c2, diff, e, n)
    print("m1:", m1)
    print("m2:", m1 + diff)

```
代码参考了[KBThe0phi1us](https://kbthe0phi1us.github.io/2023/02/16/coppersmith%E7%9B%B8%E5%85%B3%E6%94%BB%E5%87%BB/index.html)   


## many $n$   
