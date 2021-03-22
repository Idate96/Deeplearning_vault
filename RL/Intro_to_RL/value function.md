#definition 
The value function of state s under policy $\pi$ is the expeced return when starting from s and following $\pi$:

$v_{\pi}(s) = E_{\pi}[G_t | S_t = s]$

It is related to the [[Q-Function]] by taking it's expectation over the possible set of actions under [[policy]] $\pi$:
$v_{\pi}(s) = \sum_a \pi(a|s) q(a, s)$
![[value_backup_action.png]]
Substituting for the defition of $q(a, s)$ we obtain a recursive definition for the value function:

$v_{\pi}(s) = \sum_a \pi(a|s) \sum_{s', r} p(s', r| a, s) (r + \gamma v_{\pi}(s'))$

which is also known as the [[Bellman equation]] for $v_{\pi}$
