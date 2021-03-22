#cs287 
#lecture 
A [[Constrained Optimization]] problem can be formulated as
$\min_x g_0(x)$
s.t.
$g_i(x) \leq 0 \ \forall i$ 
$h_j(x) = 0 \ \forall j$

but can be turned into a non contrained formulation. 

$min_x g_0(x) + \mu \sum_i |g_i(x)|^{+} + \mu \sum_j |h_j(x)| = min_x f_{\mu}(x)$

where when we don't satisfy the contraints we are penalized.

(Page 9)
This is the penalty formulation:
![[Screenshot 2020-10-31 at 17.31.17.png]]
Why not set $\mu$ very large the beginning? 

Because the optimization problem will be badly conditioned and the mu terms will dominate the merit function. 

Q:*How do we solve it?*
-[[Newton's Method]]
-[[Gradient Descent]]
-[[Trust region method]]

In the trust region method you can retain convex contraint as such (and not penalize it (page 11))

---
Page 13
We can always solve a convex problem in the inner loop by linearizing the original problem formualtion in the outer loop. 

---
Methods to solve convex problems:
- [[Elimination]]
- [[Newton's Method]]

---
Page 25

Objective:
$min_x f_0(x)$
st $f_i \leq 0$

Can we turn it into an unconstrained problem

$min_x f_0(x) + \sum_i^{m} \mathcal{I_{-}}f_i(x)$

where the indicator is zoro if the value is negative and infinity if the value is positive. 
The problem is equivalent, beacuse when the contraints are satisfied the second term is zero. If you are infeasable then the second term is infinity and it will not be declared a solution. 

But we don't like this formulation because it poorly conditioned, it does not even tell you which direction you go if you go to infinity. Can we approximate this?
We replace the indicator with a more gradual function like 
$- \log(-f_i(x))$
![[Screenshot 2020-11-19 at 15.27.49.png]]
This function though might keep you away from the boundary, can we get closer?
We can rescale the log function with $\frac{1}{t}$ the larger the t the more it's similiar to the indicator. 

--- 
Page 28 
Now we are solving a convex problem with only equality constraints. 

---
Page 32
With this method we need to have an initial feasable solution. 
You want to obtain the most feasable point (x) by trying to drive s smaller than 0 given the problem in the slides. 
This can be used to check your contraints as well. 



---
(Page 36)
[[Dual Descent]]
[[Penalty formulation]]



