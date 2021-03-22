#cs287 
#lecture 

Q:*what if we care about sampling complexity?*

DQN always tries to learn the optimal polixy. This makes the algorithm less stable particularly if the optimal policy is very different from the current policy. 

[[DDGP]] and [[SAC]] are [[Off-policy method]] on how to locally improve the current policy.

## Q-Learning
Q-value iteration will converge to the optimal value, obviouly summing all over the available states is impossible for high dimensional state spaces. 

---
*page 6*
Tabular Q learning

---
*page 8*
Q:*How to choose actions?*

---
*page 9*
This algorithms is [[Off-policy method]]. 

Problems:
- You need to explore a lot
- You have to decrease the learning rate with time but not too quickly

## DQN

[[DQN]] We need to generalize across states. 

---
*Page 18*
Supervised learning for a parametrized Q function with a boostrapped objective. 

---
*Page 19*
Since it's an off-policy algorithm we can decouple the data (stored in a buffer) collection from the training. 
The replay buffer collects $(s, a, s')$. 

There are two Q-function: because the target is computed from the Q-value, this process could be unstable. To make it more stable you can keep one of the two networks in memory and update it every C steps. 

This algorithm has succeded in putting back RL into the map in 2013

---
[[Soft-Q]] 

Look up [[Stein Variational Gradient]]

---
[[DDPG]]
This is closer to [[On policy method]] and it can be a bit more stable. 

---
[[SAC]]


The original architecture of [[DQN]] worked fine for the atari games ([[DDPG]] cannot work with discrete action but [[SAC]] yes), at the same time policy gradients methods got developed like [[PPO]].

