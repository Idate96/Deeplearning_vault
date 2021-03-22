### When do we use an [[Q-Function]] instead of a  [[value function]]?

If a model is not available is useful to find action value function rather than state value functions. With a model state value functions alone are sufficient to determine a policy: one simply looks ahead one step and chooses whichever action leads to the best combination of reward and next state. 

$\pi' = arg\,max_a q_{\pi}(s, a) =  arg\,max_a \sum_{s', r} p(s', r| a, s) (r + \gamma v_{\pi}(s'))$

Instead with action value functions (q can be estimated with MC for example)

$\pi = arg\,max_a q(a, s)$



Why [[Bellman equation]] defines an optimal policy?


## What is the difference between value iteration and Q-learning? 

