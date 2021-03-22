#chapter
It is not practically very useful since it assumes a perfect model of the world ([[environment dynamics]]) and it is computationally very expensive. Most of other methods try to obtain the same results as DP with less computation and knowledge about the world. 

## Iterative policy evaluation 
Q: How do we find the [[value function]] related a policy $\pi$?

The update rule is: 
$v_{k + 1}(s) = \sum_a \pi(a|s) \sum_{s', r} p(s', r| a, s) (r + \gamma v_{k}(s'))$

>Expected Update: t replaces the old value of s with a new value obtained from the old values of the successor states of s, and the expected immediate rewards

Recallying the [[Bellman equation]] for the value function we find that the fixed point of the update rule is $v_{\pi} = v_{k}$.

![[Iterative_policy_eval.png]]

## Policy improvement 
We can iteratively improve the [[policy]] with the following greedy update rule:

$\pi' = arg\,max_a q_{\pi}(s, a) =  arg\,max_a \sum_{s', r} p(s', r| a, s) (r + \gamma v_{\pi}(s'))$

If the new policy $\pi'$ is as good as the policy $\pi$, this implies that $v_{\pi'} = v_{\pi} and from the previous equation it follows that: 
$v_{\pi'} = \max_a sum_{s', r} p(s', r| a, s) (r + \gamma v_{\pi'}(s'))$

Which is exactly the [[Bellman equation]], which implies that the policy is optimal.


## Policy Iteration 
![[policy iteration.png]]
![[policy_iteration.png]]

## Value Iteration 
The update rule is just the bellman equation
![[value_iteration.png]]