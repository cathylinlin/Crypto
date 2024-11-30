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

> The Shortest Vector Problem (SVP): 在格`L`中找到最短非零向量（即欧几里得范数最小）

> The Closest Vector Problem (CVP): 给定 $R^m$中的向量`v`，找到格L中与之最近的向量`u`（即  $\parallel u-v \parallel$最小）

## Something important  
一个格中最短的非0向量有多长？  
Hermite's Theorem 给出了一个上界。   
> Hermite’s Theorem : 每个n维格L，都包含一个非零向量满足：  
$\parallel v\parallel \leq \sqrt{n}det(L)^{1/n}$   

这也告诉了我们怎么造格。

## Important algorithms    
### Babai’s Closest Vertex Algorithm   
用于近似解决CVP问题。   
python实现：   
```python
import numpy as np

def gram_schmidt(A):
    m, n = A.shape
    Q = np.zeros((m, n))
    R = np.zeros((n, n))
    
    for j in range(n):
        v = A[:, j]
        for i in range(j):
            R[i, j] = np.dot(Q[:, i], A[:, j])
            v = v - R[i, j] * Q[:, i]
        R[j, j] = np.linalg.norm(v)
        Q[:, j] = v / R[j, j]
    
    return Q, R

def babai(A, target):
    Q, R = gram_schmidt(A)
    m, n = A.shape
    b = np.zeros(n)
    
    for j in range(n):
        proj = np.dot(Q[:, j], target)
        b[j] = np.round(proj / R[j, j])
        target = target - b[j] * Q[:, j]
    
    return np.dot(A, b)

if __name__ == "__main__":
    A = np.array([
        [0, 1],
        [1, 0]
    ])
    target = [18.6,14.8]

    approx_shortest_vector = babai(A, target)
    print("Approximate shortest vector:", approx_shortest_vector)
```
**已测试**   

sage实现：
```sage
def babai_algorithm(lattice_basis, target_vector):
    A = matrix(ZZ, lattice_basis)

    L = IntegerLattice(A)

    target = vector(ZZ, target_vector)

    Q, R = A.gram_schmidt()  
    R = R.transpose()  
    b = vector(ZZ, [0] * A.ncols())
    for j in range(A.ncols()):
        proj = Q[:, j] * target  
        b[j] = round(proj / R[j, j])  
        target = target - b[j] * Q[:, j]  

    return A * b

if __name__ == "__main__":
    lattice_basis = [
        [2, 1],
        [1, 2]
    ]

    target_vector = [0, 0]

    approx_shortest_vector = babai_algorithm(lattice_basis, target_vector)
    print("Approximate shortest vector:", approx_shortest_vector)
```
**未测试**   

### LLL algorithm   
通过正交化基向量并检验条件以提高基向量正交性。   
通过LLL，输入一个坏基，经过正交化和短化，满足两个条件后，输出较小的正交性好的基。一般好基的第一个向量范数最小。    

```python
#矩阵,格
L = Matrix([[10, 12], [15, 19]])

#LLL
LLL_reduced_lattice = L.lll()
```

### BKZ algorithm    
LLL算法并非完美。    
BKZ算法可以给出正交性更好的基。   
```python
# 基矩阵，格
L = Matrix([[1, 2], [3, 4]])

# BKZ算法
# beta参数,指定块大小
beta = 2
BKZ_reduced_lattice = L.lattice_bkkz(beta)

```

