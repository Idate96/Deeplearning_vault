## Dual Linear Program

$\max_{\lambda} sum_s \sum_a \sum_s' \lambda(s, a) T(s, a, s')R(s, a, s')$
subject to 
for every $s' \in S$, $\sum_a' \lambda(s', a') = \mu_0(s) + \gamma \sum_s \sum_a \lambda(s, a) T(s, a, s')$

$\lambda(s, a)$ is a distribution over actions. The objective is the expected value (over state and action) of rewards in that state taking that action. 

Definiting
$\lambda(s, a) = \sum_t \gamma^t P(s_t = s, a_t = a)$
it will satisfy the constraint above. This is the intuition, the expected discounted amount of time spent in state s taking action a. 

The optimal policy:
$\pi^{*}(s) = arg, \max_a \lambda_{s, a}$
