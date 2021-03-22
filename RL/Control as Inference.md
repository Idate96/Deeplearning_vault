#cs285 
#lecture 

---
Page 3
Q: *How do we model humans*?
We assume there an utility function, and that the behaviour is nearly optimal

We can assume that a person is behaving optimally, can we figure out w.r.t. function is optimal. 

---
Page 4
Imagine if the monkey has to move the dop over another dot. 

What is going on? is it stupid?

Humans are not always optimal, but these mistakes don't matter much for reaching for the final goal. Some mistakes matter more than other. 
Natural behaviour is also stochastic.

---
Page 5

Since behaviour is stochastic, we need a probabilistic model. A very powerful tool is probabilistic graphical model, such that inference on the model will result in almost optimal behaviour. 

The scirpt O, are optimality variables that are observed. The modelling choice is that these variables are binary, 1 if monkey is trying to optimal, 0 otherwise. 

We will set the probability of O being true to the exponential of the current reward. Rewards must be always negative. 

$p(\tau|O_{1:T}) = \frac{P(O_{1:T}|\tau) p(\tau)}{p(O_{1:T})}$

We can ingnore the prior probability over the optimal variable to rewrite it as 

$p(\tau|O_{1:T}) \approx p(\tau) \exp (\sum_t r(r_t, a_t))$

Trajectories that are suboptimal are still possible but their probability decrease exponetially with the descrease in reward.

---
Page 7
Three operations:
1. Backward messages: it's a chain structured dynamical bayes net, so it's amanable to [[Message Passing]]. The backward message tells you the probability that is the trajectory is optimal from here onwards. 
2. We can compute the policy
3. compute forward message: what's the probability of landing in state $s_t$ if you are optimal through time step $t-1$

## Control as Inference 
*page 10*
We want to obtain a recursive expression for $\beta$.

---
*page 11*
Casting this backward passing with familiar names. 


## Talk 
Let's choose the policy to be proportional the the exponential of the Q-function 

The objective here is get reward but try to be as random as possible.
![[Screenshot 2020-12-03 at 15.30.52.png]]

This though has a deeper connectoin with inference and [[Graphical models]]
![[Screenshot 2020-12-03 at 15.31.29.png]]

Our evidence is that the optimality variable is true at every time-step, in this way you can show that this graphical model implies max-entropy RL. 

In the last line you recover the negative of the RL objective
![[Screenshot 2020-12-03 at 15.35.33.png]]

Q:*How do we maximize this objective?*
1. on-policy gradient 
2. Q-learning -> soft Q-Learning 

There is also a connectoin with message passage. 
![[Screenshot 2020-12-03 at 17.07.00.png]]


[[SQL]] and [[SAC]] are related algorithms. 