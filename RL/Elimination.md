Here we are in the regime, where we have less equation than unknowns.
The solutions of $Ax = b$ take the form of
$x = \hat{x} + Fz$
where $F = span(\mathcal{N}(A))$, F span the [[Null Space]] of A . 
![[Screenshot 2020-11-19 at 11.38.44.png]]


The last rows of V^{T} or the last columns of V are the null space of A, since changing their values does not change the values of b (given by $A x= b$)
![[Screenshot 2020-11-19 at 12.56.43.png]]

Cons: In most cases the matrix A will be sparse, while F will be dense. So even though you reduce the dimensionality it might require still a lot of computation. 

