Problem: 
$max_x f(x)$ subject to $g(x) = 0$

We can solve it with a [[Lagrangian approach]], the above problem is equivalent to solving the following problem

$max_x min_{\lambda} L(x, \lambda) = f(x) + \lambda g(x)$

Let's say that for a certain $x_i$ such that $g(x_i) \neq 0$, then the [[lagrange multiplier]] $\lambda$ will be either plus or minus inifinity to drive the value of the functional down. So if we want to maximize the functional over x, we are forced to pick an x such that $g(x) = 0$, which gives back the original formulation. 

It's not more complicated because we can say that at the solution:
$\frac{\partial L}{\partial x} = 0$ 
$\frac{\partial L}{\partial \lambda} = 0$

--- 
Another formulation of a contrained optimization problem

$\min_x g_0(x)$
s.t.
$g_i(x) \leq 0 \ \forall i$ 
$h_j(x) = 0 \ \forall j$
First contrained means that you have to be inside the function, the second you have to be on the function (like a plane or sinusoid).



