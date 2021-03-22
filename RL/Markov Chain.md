A set of states M = {S, T}
S = state space (discrete or continous)
T = transition operator or $p(s_{t + 1}| s_t)$

T is an operator becasue we can express it as a a matrix to express the chain rule of probability. 

![[Screenshot 2020-10-27 at 11.36.52.png]]

The states also have the markov property. 

A markov chain can also be expressed as:
$p(\tau) = p_{\theta}(s_1, a_1, ..., s_T, a_T) = p(s_1) \prod_{t =1 }^T \pi_{\theta}(a_t|s_t)p(s_{t + 1}|s_t, a_t)$
where $\tau$ stands for the trajectory. 