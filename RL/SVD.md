**This is probably the most important concept in linear algebra**

For every mxn matrix factors into $A = U \Sigma V^{T}$.

U is a $m \times m$ [[orthogonal matrix]], $\Sigma$ a $m \times n$ diagonal matrix and V a $n \times n$ orthogonal matrix. 

Intuitively: rotation $\times$ stretching $\times$ rotation

We have already seen a decomposition of a matrix into it's eigenvectors and a diagonal matrix containing the eigenvalues ([eigenvalue decomposition](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix)), what is the difference here?

The key difference here is that U and V are different matrices and they are orthogonal matrices, and this process can be done for rectangular matrices. 
The values of in the diagonal are called *singular values* and the vectors in the other two matrices are called *singular vectors*.

Let's look at 
$A^{T}A = (V \Sigma^{T} U^{T}) U \Sigma V^{T} = V (\Sigma^{T} \Sigma) V^{T}$
and we reach the usual formulation with the eigenvalues in $\Sigma^{T} \Sigma$ and the eigenvectors in V (eigenvectors orthogonal because A^{T}A is symmetric). The eigenvalues are singular values squared.  

If we look
$AA^{T} = (U \Sigma^{T} V^{T}) V \Sigma U^{T} = U (\Sigma^{T} \Sigma) U^{T}$
so U is the eigenvector matrix for $AA^{T}$, with the same eigenvalues as before. 

## Example

$$
A = \begin{bmatrix}
		2 & 2 \\
		1 & 1 \\
		\end{bmatrix} = 
		\frac{1}{\sqrt{5}} \begin{bmatrix}
		2 & 1 \\
		-1 & 2 
		\end{bmatrix}
		\begin{bmatrix}
		\sqrt{10} & 0 \\
		0 & 0 
		\end{bmatrix}
		\frac{1}{\sqrt{2}}
		\begin{bmatrix}
		1 & 1 \\
		1 & -1 
		\end{bmatrix}
		
$$

In this example the first vector of U and V and the first value of $\Sigma$ are the important ones. 


## Application
[[PCA]] is one of the main applications of SVD. 