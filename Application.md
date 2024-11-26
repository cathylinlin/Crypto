# Application of Lattice
## NTRU    
### the simplest case   
1. pick prime `p`,`g`,random number `f` ,  $g<f<p ,gcd(f,g)=1$   
2. let $h\equiv g\cdot f^{-1}\pmod p$
3. pick random number `r`, $rg+mf<p$
4. encrypt: message `m` ciphertext `c`  $c\equiv rh+m \pmod p$
5. decrypt: let $a\equiv cf\pmod p,m\equiv (a\pmod g)\cdot f^{-1}\pmod p$

解密的正确性：   
$a\equiv cf\equiv frh+fm\equiv frgf^{-1}+fm\equiv rg+fm\pmod p$   
$rg+mf<p\Rightarrow a=rg+fm$   
$m\equiv (a\pmod g)\cdot f^{-1}\pmod p$   

------------------------

alaways, we just have `h` and `p` .   
we can use lattice:   

$h\equiv g\cdot f^{-1}\pmod p$   
$hf\equiv g\pmod p$   
$fh-kp=g$   
(do not forget f=f)   

let's create a lattice:  

$$ \begin{bmatrix} f & -k \\ \end{bmatrix} \cdot 
\begin{bmatrix} 
1 & h \\
0 & p \\
\end{bmatrix} 
= \begin{bmatrix} f & g \\ \end{bmatrix} $$   

but,why this?And how to solve?   
显然，当我们得到f时，密码几乎就被我们破译了。  
（待续


### its true form   

## pack    
## LWE   
## HNP   
## others    
### 1   
一个很好的例子。   
设n=pq,然后构造了一个矩阵p $ q // ? $ ? // 问号是比p,q比特位数长好几倍的大数。   
然后造了一个随机2*2矩阵，和前面的p,q矩阵相乘，输出结果。   
很明显的，对结果矩阵用LLL()就能求出p,q来。   
（待续
