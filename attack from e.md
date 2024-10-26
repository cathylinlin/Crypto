# Attack from e   
这次我们讲讲针对e的攻击  

## $e=2$  
直接开平方，或者加一些 $n$ 上去开平方，或者  
### Rabin 算法   
Rabin算法其实是一个加解密过程。  
加密：  
取两素数满足 $p\equiv q\equiv 3\pmod{4}$ ， $n=pq$ ，私钥为 $(p,q)$ ，公钥为 $n$   
$c\equiv m^2\pmod{n}$   

解密：  
$n=pq$  所以有   
$c\equiv m^2\pmod{p}$   
$c\equiv m^2\pmod{q}$   
根据欧拉判别条件，有 ${c}^{\frac{p-1}{2}}\equiv 1\pmod{p}$  
$c\equiv c\cdot {c}^{\frac{p-1}{2}}\equiv {c}^{\frac{p+1}{2}}\pmod{p}$   
故 $m_p\equiv {c}^{\frac{p+1}{4}}\pmod{p}$ 当然 $m_p\equiv p-{c}^{\frac{p+1}{4}}\pmod{p}$   
同理计算出 $m_q$ 一共四个，使用中国剩余定理（CRT）求出模 $n$ 下的4个根。   
至于哪个是明文，就看题目了。  
```
def rabin_attack(c, n, p, q):
    c1 = pow(c, (p+1)//4, p)
    c2 = pow(c, (q+1)//4, q)
    cp1 = p - c1
    cp2 = q - c2

    t1 = invert(p, q)
    t2 = invert(q, p)

    m1 = (q*c1*t2 + p*c2*t1) % n
    m2 = (q*c1*t2 + p*cp2*t1) % n
    m3 = (q*cp1*t2 + p*c2*t1) % n
    m4 = (q*cp1*t2 + p*cp2*t1) % n

    return m1, m2, m3, m4

mlist = rabin_attack(c, p*q, p, q)

for m in mlist:
    print(long_to_bytes(m))
```
代码参考了[NSS](https://www.nssctf.cn/index) [ctf wiki](https://ctf-wiki.org/crypto/asymmetric/rsa/rsa_e_attack/#rsa-rabin)

### Wiener Attack  
维纳攻击的细节我们留到 $e$ 非常大的时候再讨论。
实际上对于 $e$ 非常小时，维纳攻击几乎求不出 $e$ ，求出来也只有高位符合（接近一半的高位），但  $\varphi$ 却都能求出高位（接近一半的高位）  

## $e=3$  






