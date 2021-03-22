Policies are parametrized directly: $\pi(a|s, \theta)$

Properties:
- $\pi(a|s, \theta) > 0$ for all $a \in A(s)
- $\sum \pi(a|s, \theta) = 1$

We can use a Softmax-Policy parametrization to normalize the action preferences. 
![[Screenshot 2020-10-22 at 15.26.51.png]]

Preferences are not action values and are invariant under adding or subtracting a constant. So do not confute it with $\epsilon$-greedy policies over action values. 

Advantages compared to [[Function Approximation]] of [[Q-Function]]:
- Policy can be made more greedy over time autonomously, it naturally converges to a deterministic policy 
- the optimal policy might be stocastic like in the following scenario 
![[Screenshot 2020-10-22 at 15.29.24.png]]
- policy can be less complex than the value function

## Learn the policy 
Maximize the [[average reward]], via gradient ascent: 
$\nabla r(\pi) = \nabla \sum_s \mu_{\pi}(s) \sum \pi(a|s) q(a, s)$

Modifiying the policy changes the distribution $\mu$ so it's very hard to estimate the gradient. 

The [[policy gradient theorem]] allows to compute the gradient from experience:
$\nabla r(\pi) = \sum_s \mu_{\pi}(s) \sum \nabla \pi(a|s) q_{\pi}(s, a)$

*How do we approximate the gradient?*
We want an unbiased estimate. 

$\nabla r(\pi) =E_{\mu} (\sum_a \nabla \pi(a|s, \theta) q_{\pi}(s, a))$

Since we sample from the policy $\pi$ the $E_{\mu}$ is already accounted for. 
Now can rewrite the sum over action as an expectation over the policy to able to use data from experience (the one action taken):

$\sum_a \nabla \pi(a|s, \theta) q_{\pi}(s, a)) = \sum_a \pi(a|s, \theta) \frac{1}{\pi(a|s, \theta)} \nabla \pi(a|s, \theta) q_{\pi}(s, a) = E_{\pi}[\frac{1}{\pi(a|s, \theta)} \nabla \pi(a|s, \theta) q_{\pi}(s, a)]$

Therefore the update is 

$\theta_{t + 1} = \theta_{t} + \alpha \frac{ \nabla \pi(a|s, \theta)}{\pi(a|s, \theta)} q_{\pi}(s, a) = \theta_{t} + \alpha ln(\nabla \pi(a|s, \theta)) q_{\pi}(s, a)$

We can compute the gradient of the policy (we parametrize it) and the [[Q-Function]] can computed for example with a [[Temporal Difference]] algorithm.









