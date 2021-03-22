
#chapter 

## Episodic Sarsa 

Stacked features:

![[Screenshot 2020-10-22 at 12.04.32.png]]

With a NN you can generate multiple outputs one for each action value (similiar to stacking procedure).

Otherwise if we want to create an approximate function of states and actions that we can give both as input to the NN. 


![[Screenshot 2020-10-22 at 12.07.55.png]]

Update for Sarsa
$w_{t + 1} = w_t + \alpha (R_{t + 1} + \gamma q(S_{t + 1}, A_{t + 1}) - q(S_t, A_t))) \nabla q(S_t, A_t))$

## Expected Sarsa and Q-Learning
Exp Sarsa:
$w_{t + 1} = w_t + \alpha (R_{t + 1} + \gamma \sum_{a'} \pi(a'|S_{t + 1}) q(S_{t + 1}, a') - q(S_t, A_t))) \nabla q(S_t, A_t))$

Q-Learning:
$w_{t + 1} = w_t + \alpha (R_{t + 1} + \gamma max_{a} q(S_{t + 1}, a) - q(S_t, A_t))) \nabla q(S_t, A_t))$


## Exploration under function approximation 
[[Optimistic Initial Values]] are used to systematically explore the action-value space. 

Q: How do you initialize the weight of a NN? 

A NN will also loose it's optimism relatively fast (since changing of weights influences almost all of the states). 
In this course we use $\epsilon$-greedy policy. 





