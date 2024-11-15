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
## many $n$   
