# Application of Lattice
## NTRU    
### the simplest case   
1. pick prime `p`,`g`,random number `f` ,  $g<f<p ,gcd(f,pg)=1$   
2. let $h\equiv g\cdot f^{-1}\pmod p$
3. pick random number `r`, $rg+mf<p$ （m<g）
4. encrypt: message `m` ciphertext `c`  $c\equiv rh+m \pmod p$
5. decrypt: let $a\equiv cf\pmod p,m\equiv a\cdot f^{-1}\pmod g$

解密的正确性：   
$a\equiv cf\equiv frh+fm\equiv frgf^{-1}+fm\equiv rg+fm\pmod p$   
$rg+mf<p\Rightarrow a=rg+fm$   
$m\equiv (a\pmod g)\cdot f^{-1}\pmod g$   

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
首先`1,0,h,p`不是好的格基，其中`h`和`p`是几乎一样大的。接着我们知道这个格可以表示出含`f,g`（我们要求的就是f,g）的一组基，而且根据加密过程,`f,g`是远小于`h,p`的。这就近似于SVP问题。   
而LLL算法刚好是一个可以近似解决SVP问题的好算法。那自然可以借此大概率求出`f,g`来。  
也就完成了解密。   

-----------------------------

稍难一点的题目则会出现`f,g`并非最短向量的情况，那我们就需要在格和基都配上一个 `D（=2**？）`，使格的行列式大于含`f,g`的基的范数。这也就是“配平”。   



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
