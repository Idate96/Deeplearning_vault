1-step problem:
$max_{\pi(a)}E[r(a)] + \beta H(\pi(a))$
where H stand for the [[entropy]]. this is equivalent to:
$max_{\pi(a)} \sum_a \pi(a) r(a) - \beta \sum_a \pi(a) log(\pi(a)$
such that $\sum_a \pi(a) = 1$

The Lagrangian is:
$max_{\pi(a)} min_{\lambda} L(\pi(a), \lambda) =  \sum_a \pi(a) r(a) - \beta \sum_a \pi(a) log(\pi(a) + \lambda (\sum_a \pi(a) - 1)$

Now we can take the gradient of the lagrangian over the policy:
$\frac{\partial L}{\partial \pi(a)} = r(a) - \beta log \pi(a) - \beta + \lambda = 0$
This implies that:
$\pi(a) = exp(\beta^{-1}(r(a) - \beta + \lambda))$
Since $\beta$ and $\lambda$ are just scaling we can rewrite it as:
$\pi(a) = \frac{1}{z} exp(\beta^{-1}r(a))$

Now by taking the gradient w.r.t the lagrangian multiplier:
$\frac{\partial L}{\partial \lambda} = \sum_a \pi(a) - 1 =  0$

This fixes the value of the parameter $z$ to:
$z = \sum_a exp(\beta^{-1}r(a))$

Now we can take the solution for $\pi(a)$ and plug it in the objective to find the maximum value that we obtained:
$V = \beta log sum_a exp(\frac{1}{\beta}r(a))$
which is a softmax on the policy. 

## Value iteration 
Replace $V_{\pi}$ with $Q(s, a)$.

You need to bring your entory down a lot! 
