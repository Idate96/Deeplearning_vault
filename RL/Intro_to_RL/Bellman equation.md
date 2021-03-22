
Bellman equation for the [[value function]]:

$v_{\pi}(s) = \sum_a \pi(a|s) \sum_{s', r} p(s', r| a, s) (r + \gamma v_{\pi}(s'))$

Bellman optimality equation for the [[value function]]:

$v_{*}(s) = \max_{a \in A(s)} q_{\pi*}(a, s) = \max_a \sum_{r, s'} p(s', r| a, s)(r + \gamma v_{*}(s'))$

and the corresponding one for the [[Q-Function]]

$q_{*}(s, a) = \sum_{r, s'} p(s', r| a, s)(r + \gamma \max_{a'} q_{*}(s', a'))$