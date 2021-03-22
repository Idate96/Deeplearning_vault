## Multi armed bandits
#chapter 

It's a classical problem in RL. The agent maximized its rewared by choosing between a set of action. Each action is associated with an [[action-value]].  


This is an example of a 10-armed bandits problem with stationary probability distributions:
![[10_armed_bandits.png]]

In this case we assume that the action-value function is computed by averaging rewards and that the agent uses a [[epsilon-greedy-action]].

![[bandit_algo.png]]

If constant update parameters $\alpha$ is chosen is reccomended to use [[Optimistic Initial Values]]

