Original problem:
$\min_x g_0(x)$
s.t.
$g_i(x) \leq 0 \ \forall i$ 
$h_j(x) = 0 \ \forall j$

$min_{\lambda \leq 0, \nu} \max_x g_0(x) + \sum_i \lambda_i g_i(x) + \sum_j \nu_j h_j(x)$
If contrained are satisfied $g_i(x) < 0$ therefore the the best $\lambda_i \leq 0$ to minimize the problem is $\lambda_i = 0$. 
If contrained are violated, we can take a very negative lambda (minimizer), so since we are maximizing over x the optimizer will try to satisfy the constraint. Same old for v. 

Dual descent iterates:
- optimize over x (fixed $\lambda$ and $\nu$)
- Gradient descent step for $\lambda$ and $\mu$

This is a very cheap update over the lagrange multipliers:
$\lambda_i = \lambda_i + \alpha g_i(x)$
$\nu_i = \nu_i + \alpha h_j(x)$