#definition 
The expected return when taking action a, while in state s and following policy $\pi$

$q(a, s) = E_{\pi}[G_t| A_t = a, S_t = s]$

It can be expressed in terms of the value function:
$q(a, s) = E_{\pi}[R_t + \gamma G_{t + 1}| A_t = a, S_t = s] = \sum_{s', r} p(s', r| a, s) (r + \gamma E_{\pi}[G_{t + 1}| A_t = a, S_t = s])$

Substituting with the definition of [[value function]]:
$q(a, s) = \sum_{s', r} p(s', r| a, s) (r + \gamma v_{\pi}(s'))$

The Q-function can be defined as 
$Q^{\pi}(s_t, a_t) = \sum_{t' = t}^{T} E_{\pi_{\theta}}[r(s_{t'}, a_{t'}|s_t, a_t)]$