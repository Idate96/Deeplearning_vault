#cs287 

A soft model of optimality is often really good to model human behaviour. 

---
*Page 4-5*
Motivation for learning rewards. 
It sometimes very hard to imitate humans, and their actions. On the other hand we can copy the intent (like humans do) and this can lead to the same outcome. 

In many of the RL task we want to tackle, the reward is very uncertain and less obvious (like in driving). 

---
*Page 6*
Inverse RL is very underspecified problem, there are infinitely many reward function that fit this problem. 

---
*Page 7*
We get sampled trajectory from an unknown optimal policy $\pi^{*}$. We want to learn $r_{\psi}(s, a)$. 
We can have a linear parametrization but also a NN parametrization. 

---
*Page 8*
Features can be, overtaking from left, speed of the car and so on... 

The trouble with this formulation, is that are so many policies.

---
*Page 9*
Maximum margin problem that is difficult to solve gets reformulated into another optimization problem that is equivalent. As the policies become more different from the optimal policy you have to find a bigger margin m. 

---
*Page 13*
We learn the reward function using these probabilistic graphical models. 
We learn the optimality variable by doing maximum likelihood. 

The log Z is the hard part and it's assigning higher reward to probabilities that you so and lower probabilities to trajectories you did not observe. 

---
*Page 15*
Q: *How do we estimate the gradient?*






# Deep RL bootcamp


## Maximum entropy inverse RL 

The probability of a trajectory under the expert is exponential with the reward:
$p(\tau) = \frac{1}{Z}exp(R_{\psi}(\tau))$

Then we maximize the log-likelihood of the trajcetory wrt the parameters $\psi$, the tricky part is the partition function. 

---
*Page 6*
Derivation of the maximum likelihood formulation 

---
*Page 7*


*GOAL $p(s|\psi)$: state visitation frequencies*
The state visitation frequency for an episode is 
$p(s|\psi) = \sum_t \mu_t(s)$
where
$mu_t(s): prob of visiting state s at time t$

Which can be obtained with [[Dynamic Programming]]
$mu_1 = p(s_1 = s)$
for t = 1 : T:
	$\mu_{t + 1}(s') = \sum_a \sum_s \mu_t(s) \pi(a|s)p(s'|s, a)$

This works only on low dimensional spaces. 

If the dynamics are unknown we can sample Z, 

















