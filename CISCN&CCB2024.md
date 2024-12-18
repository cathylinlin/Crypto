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
1. bytes_to_long()函数相当于把字符串转256进制。256个数值取自扩展ASCII码。比如`b"ABC"`转长整型的结果就是 $65\times 256^2+66\times 256+67$
2. `&(2**128-1)`等价于 `mod 2**128`
3. 鉴于`i`在正负256之间，`^=i`可以看作 $+r_i$ ,`r_i`可以理解为是一个小误差。

于是，整个编码过程可以看作： $b_i\equiv xb_{i-1}+r_{i-1} \pmod{2^{128}} , 0\leq i\leq n$  (  `b0`就是base_num)     
我们知道`b_n,x`,要求所有`r_i`(提一嘴，有点像LCG)   


怎么做呢，造一个格。  
首先要确定总关系式，我们把上面的递推过程整理一下，就是： $b_n\equiv x^nb_0+x^{n-1}r_0+\cdots +r^{n-1} \pmod{2^{128}}$  
接着造格L：  

the lattice:  

$$
\begin{bmatrix} 
1 & 0 & 0 & \cdots & 0 & 0 & x^{n-1} \\
0 & 1 & 0 & \cdots & 0 & 0 & x^{n-2} \\
0 & 0 & 1 & \cdots & 0 & 0 & x^{n-3} \\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots \\
0 & 0 & 0 & \cdots & 1 & 0 & 1 \\
0 & 0 & 0 & \cdots & 0 & 1 & x^nb_0-b_n \\
0 & 0 & 0 & \cdots & 0 & 0 & MOD \\
\end{bmatrix} 
$$

我们用 $(r_0,r_1,\cdots ,r_{n-1},1,k)$与之相乘，得到 $(r_0,r_1,\cdots ,r_{n-1},1,0)$  (`k`是什么？`k`是用来处理同余的。 $b_n=\cdots +k\cdot 2^{128}$)   
不过也有师傅在格最后一行添加(0,0,...,2**256)的，我没看懂，可能是为了配平吧。  

然后就是配平。 $\sigma (L)$ 大约就是 ${(2^{128})}^{1/(n+2)}$，目标向量的范数大约(都取128计) ${(n128^2)}^{1/2}$ 
`n`大约20（giao数字个数除以2），显然是不够的。通过对其他师傅wp的学习，我们可以这样配平：乘一个对角矩阵。  
对角矩阵的对角线元素如下： $([2^8] * n + [2^{16}] + [2^{256}])$ (**注意**： 配平的矩阵要和你造的格相匹配，格基规约后还要除掉配平矩阵)

再然后格基规约，选择最后两项为1，0的基，这样就得到了所以`r_i`（**注意**： 规约基的选择要和你造的格相匹配）  
最后带回原编码过程，得到字符串的所有字符的（转长整型后的）十进制表示。
再处理一下即可。
脚本如下：

```python
#用sage

b0 = 0x6c62272e07bb014262b821756295c58d
x = 0x0000000001000000000000000000013b
MOD = 2**128
giao=201431453607244229943761366749810895688

n=20

#造格
Ge=Matrix(ZZ,n+2,n+2)
for i in range(n):
    Ge[i,i]=1
    Ge[i,-1]=x^(n-i-1)
Ge[-1,-1]=MOD
Ge[-2,-1]=x**n*b0-giao
Ge[-2,-2]=1

#配平
Q = Matrix.diagonal([2^8] * n + [2^16] + [2^256])  
Ge *= Q

#规约
Ge=Ge.BKZ()
Ge /=Q

ans=[]
for i in range(n+2):
    if Ge[i,-1]==0 and Ge[i,-2]==1:
        ans=Ge[i]

print(ans)         #(-29, -50, 0, -27, -22, -19, 34, -11, 3, 0, 31, -47, -16, -26, 20, 26, 6, -39, 11, -22, 1, 0)

b = 0x6c62272e07bb014262b821756295c58d  #是b0
res=[]
for i in range(n):
	b = (b* x) %MOD
	res.append(b^(b+ans[i])%MOD)  #这里不要用sage  
	b= (b+ans[i])%MOD
print(res)         #[29, 242, 0, 109, 46, 51, 98, 21, 61, 0, 31, 83, 16, 42, 124, 42, 10, 89, 21, 22]

hex_string = ''.join(f'{num:02x}' for num in res)
print(hex_string)  #1df2006d2e3362153d001f53102a7c2a0a591516
```






































