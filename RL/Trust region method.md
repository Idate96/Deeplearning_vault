This is often a very popular optimization procedure in control. 
We are trying to solve:
$min_x g_0(x) + \mu \sum_i |g_i(x)|^{+} + \mu \sum_j |h_j(x)| = min_x f_{\mu}(x)$


$\mu = 1$
While(1)
We use the linearized objective 
$min_x g_0(\bar{x}) + \nabla_x g(\bar{x})^T(x - \bar{x}) + \mu \sum_i |g_i(\bar{x}) + \nabla g_i(\bar{x})^T(x - \bar{x})|^{+} + \mu \sum_j |h_j(\bar{x} + \nabla f_i(\bar{x})^t(x - \bar{x}))|$
This problem now is **convex**
subject to $||x - \bar{x}||_2  < \epsilon$, this is called a trust-region objective (easy contraint to deal with).
You solve the optimization problem and set $x = \bar{x}$ and you iterate until convergence in the inner loop.
After it converges you increase $\mu = t \mu$ and repeat. 

Algorithm:
![[Screenshot 2020-10-31 at 17.36.05.png]]


