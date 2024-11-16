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

## many $n$   
