# python    
##  1    
### 1.1    
product(a,b)函数   
product(‘0123456789’, repeat=5)    
对a中的数选择b个进行排列组合，可重复，情况会全列出来    
来自自带库itertools   

### 1.2   
reduce() 函数相当于迭代。    
函数将一个数据集合中的所有数据进行下列操作：用传给 reduce 中的函数 func（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 func 函数运算，最后得到一个结果。   
```
reduce(func, iterable[, initializer])   
```

### 1.3   
求离散对数   
sympy库discrete_log()函数   
例如：   
```
x = sympy.discrete_log(41, 15, 7)  
print(x)
#x=3
```
即是 7^3 ≡ 15 (mod 41)   

### 1.4     
sage 的一些函数用法   
```
gcd(x,y) #求(x,y)
xgcd(x,y) #求(x,y) 返回ax+by=(x,y)的一组解
a = divisors(e)  #返回e的因子列表
n.factorial()  #求n!
crt([a_0,a_1,...,a_i-1],[m_0,m_1,...,a_i-1])  #CRT $x\equiv a_i \pmod{m_i}$
legendre_symbol(d,p) #求(d\p)
jacobi_symbol(d,P)   #求(d\P)
Zmod(m)(a).multiplicative_order() #$a^d\equiv 1\pmod{m}$ 求d
Zmod(m)(a).log(g)  #$g^?\equiv a\pmod{m}$ 求?
primitive_root(n)  #求原根
Zmod(m)(a).nth_root(n)  #求 $x^n\equiv a\pmod{m}$ 的根
P.<x>=PolynomialRing(Zmod(n)) #构造Z上的关于x的整系数多项式
#f=....
f=f.monic()   #转化为首一多项式
f.coefficients()  #返回多项式系数列表

#$a^x\equiv b\pmod{n}$
x=discrete_log(mod(b,n),mod(a,n))

x=f.small_roots(x.bits.beta=0.4[,epsilon=0.02]) #求解$f(x)=0\pmod{n}$ beta可以取0.3 后面那个暂时不知道
```
### 1.5    
bytes_to_long()等价于转256进制。再转二进制，位数就x8。    

### 1.6    
求 $m^e\equiv c\pmod{n}$ $e$ 又不大不小？    
可以用sympy库里的 nthroot_mod()函数   
```
m = nthroot_mod(c,e,p)
```
当然也可以用sage解方程    
```
# c = m ^ 256 mod p
P.<x> = Zmod(p)[]
f = x ^ 256 - c
a = f.roots()
for i in a:
    print(long_to_bytes(int(i[0])))
```
参考了[emmaaaaaaaaaa](https://blog.csdn.net/XiongSiqi_blog/article/details/130835128)
















