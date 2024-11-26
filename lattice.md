# lattice    
## what's lattice?    
(括号内问题我会择日讲解。)    
格(lattice)可以理解为整系数向量空间(vector space) (what's vector space?)     
一组线性无关的，可以张成整个向量空间的向量集称为该向量空间的基。(什么是线性无关？什么是张成？)     
基向量之间越垂直，其正交性越好。   
格是 $R^n$ 的基的全部整系数线性组合。   
这组线性无关的向量我们记为B.    
格不妨记为   

$$\mathbf{L}(B)=\{\sum_{i=1}^{n} x_i \mathbf{b}_{i}:x_i\in\mathit{Z},i=1,2,...,n\}$$   

格是加法群(什么是加法群？)    
格是偏序集(什么是偏序集？)    

## lattice's problems    
CVP,SVP,BDD,GGH,(R)LWE,HNP,HSSP,HLCP...    

## Important algorithms    
### LLL algorithm   
通过正交化基向量并检验条件以提高基向量正交性。   
通过LLL，输入一个坏基，可以输出较小的正交性好的基。一般好基的第一个向量范数最小。    

```
#矩阵,格
L = Matrix([[10, 12], [15, 19]])

#LLL
LLL_reduced_lattice = L.lll()
```

### BKZ algorithm    
LLL算法并非完美。    
BKZ算法可以给出正交性更好的基。   
```
# 基矩阵，格
L = Matrix([[1, 2], [3, 4]])

# BKZ算法
# beta参数,指定块大小
beta = 2
BKZ_reduced_lattice = L.lattice_bkkz(beta)

```

