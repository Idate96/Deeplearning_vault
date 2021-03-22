Given a function $f(x)$ we can approximate it with 
$f(x + \Delta x) = f(x) + \nabla f(x)^T \Delta x + \frac{1}{2} \Delta x^T \nabla^2 f(x) \Delta x$
In convex function $\nabla^2 f(x) > 0$ is [[positive definite]]

The minimum of the function is 
$\Delta x* = -(\nabla^2 f(x))^{-1} \nabla f(x)$

The algorithm is
![[Screenshot 2020-10-31 at 11.07.40.png]]
$\lambda$ measure the progress. 

This method is [[affine invariant]].
![[Screenshot 2020-10-31 at 11.09.58.png]]

## Newton for convex optimization problems 
If the gradient of the function is orthogonal to the plane defined by $Ax = b$. 

If with a change of x the problem is still feasable then the gradient should be orthogonal to find the optimal solution. 

$\nabla f(x^{*}) + A^{T}\nu = 0$
This optimality condition is sayin gthat the gradient of the function has to point in a direction orthogonal how we can move in the plane. $Ax = b$ is saying in the direction A you have to be a distance b out (from the origin), all the points at distance b in that direction form the line (the vector A is perpendicular to the line defined by $Ax = b$). 
When there are multiple constraints, multiple plains, you want the gradient to be pointing in the direction of any linear combination of the matrices $A_i$ (which define what is orthogonal to a line). 

Q:*What is $\nu$*?

![[Screenshot 2020-11-19 at 13.01.40.png]]


![[Screenshot 2020-11-19 at 14.52.23.png]]


![[Screenshot 2020-11-19 at 14.52.58.png]]


There is also an infesible start variant (feasable iff $Ax = b$) for the algorithm. 