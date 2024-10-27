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
但是Rabin算法还是有条件的，即要求私钥要模4余3，如果是普通的素数又该如何呢？下面介绍几个算法。  

### Cipolla 算法  
Cipolla算法用来解决 $x^2\equiv a\pmod{p}$ 的通解。其中 $a$ 是 $p$ 的二次剩余。   
**算法**：构造 ${\omega}^{2}\equiv k^2-a\pmod{p}$ ,其中 ${\omega}^{2}$ 是 $p$ 的二次非剩余。于是解为： $x\equiv{(\omega+k)}^{\frac{p+1}{2}}\pmod{p}$    

**证明**：先证明 ${(a+b)}^{p}\equiv a^p+b^p\pmod{p}$ ,这是显然的，因为将其二项式展开，中间项都有含p的系数。  
于是 $x^2\equiv {(\omega+k)}^{p+1}\equiv {(\omega+k)}^{p}\cdot(\omega+k)\equiv ({\omega}^{p}+k^p)\cdot(\omega+k)
\equiv(\omega\cdot{({\omega}^{2})}^{\frac{p-1}{2}}+k)\cdot(\omega+k)
\equiv (-\omega+k)\cdot(\omega+k)\equiv k^2-\omega^2\equiv a\pmod{p} $     
其中用到了欧拉定理和二次剩余的知识。   
```
def Cipolla(n,p):
    # 如果n不是p的二次剩余 报错
    if testResidual(n,p) == False:
        raise Exception('No solution')

    # 随机取w^2
    import random
    while True:
        a = random.choice(range(1,(p-1)//2+1)) # [1,(p-1)//2]区间内任取a 这里取半个Fp区间是因为a^2是关于(p-1)//2对称的
        w_2 = (pow(a,2)-n)%p
        if testResidual(w_2,p) == False: # 找到一个w^2 是一个二次非剩余
            break
            
    # 定义Fp2这个域 在class里实现他的运算法则
    class omega_field:

        def __init__(self,a,b):
            self.a = a%p
            self.b = b%p

        # 乘法运算
        def mul(self,another):
            x = self.a*another.a + self.b*another.b*w_2
            y = self.b*another.a + self.a*another.b
            return omega_field(x,y)

        # 乘方运算
        # 快速幂运算
        # 常规循环累乘方的复杂度是O(n) 快速幂运算则是O(logn)
        # 把指数进行了二进制分解 循环次数等于二进制的位数
        def pow(self,n):
            ans = omega_field(1,0)
            t = self
            while(n):
                if n&1:
                    ans = omega_field.mul(ans,t)
                t = omega_field.mul(t,t)
                n >>= 1
            return ans
        
        # get method
        def get(self):
            return self.a,self.b
        
    x = omega_field.pow(omega_field(a,1),(p+1)//2).get()

    # x.b理论上讲一定为0 这里保险起见加一个报错
    if x[1] != 0:
        raise Exception('Please contact the coder, something goes wrong')
    else:
        x = x[0]
    return x,p-x

```   
代码参考了[1](https://wangbard.github.io/posts/Rabin%E5%8A%A0%E5%AF%86%E5%8E%9F%E7%90%86/) [2](https://zhuanlan.zhihu.com/p/533927542)   

### AMM算法   
我们留到 attack from p,q 再讨论。

### Wiener Attack  
维纳攻击的细节我们留到 $e$ 非常大的时候再讨论。
实际上对于 $e$ 非常小时，维纳攻击几乎求不出 $e$ ，求出来也只有高位符合（接近一半的高位），但  $\varphi$ 却都能求出高位（接近一半的高位）  

## $e=3$  






