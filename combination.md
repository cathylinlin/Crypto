# 1   
## 1   
### 1.1   
$y\equiv x^p\pmod{n}$   
$n=pq$ , $x$ 与 $p$ 互素   
有： $y\equiv x\pmod{p}$    
**证明**：   
$y\equiv x^p\pmod{n} \Rightarrow y\equiv x^p\pmod{p}$     
$y\equiv x\cdot {x}^{p-1}\equiv x\pmod{p}$   

### 1.2   
如果 $d$ 和 $\varphi$ 的bit位相等，可以通过 $ed=1+k\varphi(n)$ 爆破k，继而得到 $\varphi$   

### 1.3   

## 2    
### 2.1    
一类数论推导题。    
特点是给了两个Hint,都是关于 $p,q,n$ 的同余式。我们的目标就是将这两个Hint化为模 $q$ 结果相等的模样，然后和 $n$ 求gcd.     
举个例子：    
$h_1\equiv (kp+q)^t\pmod{n}$    
$h_2\equiv (lp+x)^q\pmod{n}$    
容易化为： $h_1\equiv (kp+q)^t\equiv (kp)^t+q^t\pmod{n} \Rightarrow h_1\equiv (kp)^t\pmod{q}$   
$h_2\equiv lp+x\pmod{q}$    
于是有： $(h_2-x)^t\cdot k^t\equiv (lkp)^t\pmod{q}$     
$h_1\cdot l^t\equiv (lkp)^t\pmod{q}$   
最后gcd就可以求出 $q$ 了    

### 2.2    
一类数论推导题。   
特点是给了一个Hint,是关于 $p,q,n$ 的同余式。我们的目标还是gcd.   
举个例子：   
$h\equiv (kp+t)^q\pmod{n}$   
好的方法是去模 $p$ ，而不是模 $q$ .    
$h\equiv t^q\pmod{p}$   
注意到：  $t^n\equiv t^{p\cdot q}\equiv t^q\pmod{p}$   
之后求 $gcd(h,t^n)$ 就能求出 $p$ 了。      

### 2.3    
一类数论推导题。     
特点是给了hint，是关于 $m,n$ 的同余式。我们的目标是展开。    
举个例子：   
$h\equiv (tn^2+1)^m\pmod{n^3}$   
用二项式展开：   
$h\equiv tmn^2+1\pmod{n^3}$   
接着比较 $h,m,n$ 的比特位数，判断下一步怎么做。    

## 3   
### 3.1    
wiener attack的其他用法。   
> 任何比例非常接近另外一个已知比例情况下想到维纳攻击

一般见于给了一个含很多参数的同余式。    

## 4   
### 4.1  dp leak    
思路是gcd。    
$d_p\equiv d\pmod{p-1}$   
$ed\equiv 1\pmod{\varphi} \Rightarrow ed\equiv 1\pmod{p-1} \Rightarrow ed_p\equiv 1\pmod{p-1}$   
$a^{ed_p}\equiv a^{k(p-1)+1}\equiv a\pmod{p} \Rightarrow gcd(a^{ed_p}-a,n)=p$    

   












