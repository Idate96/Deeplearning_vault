Iterate

Collect data $(s_t, r_t, a_t, s_{t + 1})$ and add to the reply buffer

Loss: $\sum_k (y(s_k) - Q_{\theta}(s_k, a_k))$ where 
$y(s_k) = r_k + \gamma max_{a'} Q_{\theta-}(s_{k + 1}, a')$

Every few iteration set $\theta = \theta-$

## Double Q Network
Main idea is that you use one network to select the action and one to evaluate the best action. In this way we hope to have decorrelated noise and lower the overestimation of Q values (which happens because we take the max over a noisy signal).

![[Screenshot 2020-12-01 at 19.44.44.png]]

## Prioratized Experience Replay
Prioratize the samples that have a large [[Bellman equation]] error. 

![[Screenshot 2020-12-01 at 20.15.44.png]]


