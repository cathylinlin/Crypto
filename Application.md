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

use lattice:   
$h\equiv g\cdot f^{-1}\pmod p$   
$hf\equiv g\pmod p$   
$fh-kp=g$   


### its true form   

## pack    
## LWE   
## HNP   
## others   
