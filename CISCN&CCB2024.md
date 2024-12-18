# Crypto  
## rasdn  
比赛期间就做出来了，简单复盘一下，然后拓展一点。  
flag分成两部分，分别用了两种不同的方式加密。  
第一个加密方式的主要逻辑：  
```python
def crypto1():
    p = getPrime(1024)
    q = getPrime(1024)
    n = p * q
    e = 0x10001
    x1=randint(0,2**11)
    y1=randint(0,2**114)
    x2=randint(0,2**11)
    y2=randint(0,2**514)
    hint1=x1*p+y1*q-0x114
    hint2=x2*p+y2*q-0x514
    c = pow(bytes_to_long(flag1), e, n)
    print(n)
    print(c)
    print(hint1)
    print(hint2)
```
给了两个形如`px+qy`的hint，注意到`x`比较小，我们直接爆破。  
爆破出来后用 $x_1h_2-x_2h_1$（消掉了`p`，得到的是`q`的倍数）与`n`取最大公约数，得到`q`  
```python
for a1 in range(2**10,2**11):
    for a2 in range(2**10,2**11):
        x=a1*h2-a2*h1
        q=gcd(x,n)
        if q!=1:
            print(q)
```
第二个加密方式的主要逻辑：  
```python
def crypto2():
    p = getPrime(1024)
    q = getPrime(1024)
    n = p * q
    e = 0x10001
    hint = pow(514*p - 114*q, n - p - q, n)
    c = pow(bytes_to_long(flag2),e,n)
    print(n)
    print(c)
    print(hint)
```
第二个加密只给了一个hint， ${(514p-114q)}^{n-p-q}\pmod{n}$  
通过变形，得到： ${(514p-114q)}^{n-p-q} \equiv {(514p-114q)}^{\varphi (n)-1} \equiv \frac{1}{514p-114q} \pmod{n}$   
第二个同余号根据欧拉定理易得。作hint对n的模逆元，然后和`n=pq`联立解方程，就得到`p,q`
```python
t=inverse(h1,n)
p=Symbol('p')
q=Symbol('q')
f1=p*q-n
f2=514*p-114*q-t
ans=solve([f1,f2],[p,q])
print(ans)
```  

**拓展**:  
### 1  
在第一段加密中，如果给出的两个hint`px+qy`，`x`和`y`都比较大该怎么处理？  
### 2  
在 1 的基础上，给了三个hint,该怎么处理？  
### 3  
在 1 的基础上，给了30个hint,该怎么处理？  

（待续


## fffffhash  
并非逆向题，并非并非逆向题。  
但是是格密码。  

主要逻辑：求一个由 16进制二位数（即 0x00 到 0xff）组成的字符串，经过下列编码后，和目标结果(giao)一样  
```python
def giaogiao(hex_string):
	base_num = 0x6c62272e07bb014262b821756295c58d
	x = 0x0000000001000000000000000000013b
	MOD = 2**128
	for i in hex_string:
		base_num = (base_num * x) & (MOD - 1) 
		base_num ^= i
	return base_num

giao=201431453607244229943761366749810895688
```

解此题需要知道几个知识点和关键点：  
1. bytes_to_long()函数相当于把字符串转256进制。256个数值取自扩展ASCII码。比如`b"ABC"`转长整型的结果就是 $65* 256**2+66* 256+67$
2. `&(2**128-1)`等价于 `mod 2**128`
3. 鉴于i在正负256之间，`^=i`可以看作 $+r_i$ `r_i`可以理解为是一个小误差。

于是，整个编码过程可以看作： $b_i\equiv xb_{i-1}+r_{i-1} \pmod{2^{128}} , 0\leq i\leq n$  (  `b0`就是base_num)     
我们知道`b_n,x`,要求所有`r_i`  
提一嘴，有点像LCG(  
怎么做呢，造一个格。  
首先要确定总关系式，我们把上面的递推过程整理一下，就是： $b_n\equiv x^nb_0+x^{n-1}r_0+\cdots +r^{n-1} \pmod{2^{128}}$  
然后造格：  





































