# attack from m    
## m leak   
类似p leak   
$m={m}_{high}+x$   
```python
PR.<x>=PolynomialRing(Zmod(n))
f=(m_h+x)^e-c
f=f.monic()
x0=f.small_roots(2^unkownbits,1)[0]
m=m+x0
```
