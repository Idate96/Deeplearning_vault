 Assuming a smooth function 
 $f(x_0 + \delta x) = f(x_0) + \nabla_x f(x_0)^T \Delta x + O(\delta x^2)$
 
 The direction of steepest descent is given by 
 $\Delta x^{*} = arg min_{\Delta x: ||\Delta x|| \leq \epsilon}  f(x_0) + \nabla_x f(x_0)^T \delta x$
 
 Since $min_{b:||b|| \leq \epsilon} a^T b$ is given by picking b in the opposite direction of a and rescaling it to meet the norm constraint $b = \frac{-a}{||a||_2} \epsilon$
 
 Therefore
 $\Delta x = \frac{-\nabla_x f(x_0)}{||\nabla f(x_0)||_2}\epsilon$, or simply the direction steepest descent $\delta x* = -\nabla_x f(x_0)$
 
 ## Line search 
 Let's find the best step size t 
$min_t f(x + t \Delta x)$
 but this is not possible to do exactly. 
 
![[Screenshot 2020-10-31 at 10.09.16.png]]

We try some stepsize starting big, when the condition is met (unhappy), we reduce the stepsize and test again. 

![[Screenshot 2020-10-31 at 10.10.33.png]]

In a [[Convex Function]] a linaer approximation is alwasy optimistic, the $\alpha$ factor is scaling down expectations.

---
Regular grandient descent suffer from poor conditioning ([[condition number]])