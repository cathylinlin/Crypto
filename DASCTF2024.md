# Crypto  
## 数论的香氛  
先看题目内容：  
> 你嗅嗅勒让德小饼干和二次剩余咖啡，是不是很香？这里为你准备了pohlig小盘子和copper小勺

看来是纯纯数论题。  

接着看看文件内容，它的主要逻辑如下：  
一直取随机数`r1`,直到得到素数 $p=r_12^k+1$    
```python
def get_p():
    global k
    while True:
        r=random.randint(2**69,2**70)
        p=2**k*r+1
        if isprime(p):
            return p
        else:
            continue
```
一直取随机数`r`,直到得到素数 $q=4r+3$  
```python
def get_q():
    while True:
        r=random.randint(2**147,2**148)
        q=4*r+3
        if isprime(q):
            return q
        else:
            continue
```
一直取`y`,直到`y`既不是`p`的二次剩余，也不是`q`的二次剩余  
```python
def get_y():
    global n,p,q
    while True:
        y=random.randint(0,n-1)
        if legendre_symbol(y,p)==1:
            continue
        elif legendre_symbol(y,q)==1:
            continue
        else:
            return y
```
之后把flag分成5段，每段都用同样的方法加密：  
```python
def encode(m):
    global y,n,k
    x = random.randint(1, n - 1)
    c=(pow(y,m,n)*pow(x,pow(2,k),n))%n
    return c
```
最后给了`n=pq`,`y`和密文。  

首先`n`比较短，先丢factordb里面分解，没有，再丢yafu里面分解，  
然后写出密文的表达式： $c\equiv (y^m\pmod{n}\cdot x^{2^k}\pmod{n})\pmod{n}$  
即： $c\equiv y^mx^{2^k}\pmod{n}$  

到这里我就猜测 $x^{2^k}$肯定是可以去掉的，比如说是一个定值，或者可以和某个数结合。  
于是我取调试了一下，发现不是定值。  
此时刚好yafu分解出来了。得到了`p`,`q`。  
于是我就尝试去凑费马小定理的形式，一直没凑出来。  
然后就再看了一眼题干，上网搜了一下pohilg,这是一个求离散对数的方法的名字之一，特点是运用底数的不良构造。  
接着我再尝试，终于得到： $c^{r_1}\equiv y^{mr_1}x^{r_12^k}\equiv y^{mr_1}\equiv {(y^{r_1})}^m\pmod n$  


之后怎么做就显而易见了。上网检索发现sage自带的求离散对数的方法包含pohlig-hellman方法。那就赶紧去一把梭！  

```sage
from Crypto.Util.number import *
n=
y=
cs=

k=79 
p= 628729403897154553626034231171921094272614401
r1=(p-1)//2**k

cr1=[]
for i in range(len(cs)):
    cr1.append(pow(cs[i],r1,n))

yr1=pow(y,r1,n)

for i in range(len(cr1)):
    m=discrete_log(mod(cr1[i],p),mod(yr1,p))  #注意这个p
    print(long_to_bytes(m))
```

值得注意的是，如果注释那里用的是`p`的话，一下子就出结果了，但如果是`n`，要爆破很久。  
（为什么呢？我猜测就是pohlig的方法非常需要底数的不良构造，就像这里的`p`）

flag是： b'DASCTF{go0_j06!let1sm0v31n_t0_th3r_chanlenges~>_<}'   
ps: 真正的flag:
```
b'DASCTF{go0'
b'_j06!let1s'
b'm0v31n_t0_'
b'th3r_chanl'
b'enges~>_<}'
```


## 矩阵创造师  
又臭又长，直接被吓跑了。   

## the_last_challenge_attack  
又臭又长，直接被吓跑了。

















