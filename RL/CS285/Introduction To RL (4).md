#cs285
#lecture 

(Up to page 9)
 [[Markov Chain]] is part of [[MDP]]. 
 The markov decision process adds the action space and reward function. 
 
 The transition operator now is a (3) tensor: $T_{ijk} = p(s_{t + 1} = i|s_t = j, a_t = k)$
 
 [[POMDP]] are partially observation markov decision processes. 
 
 (Page 10)
 We can express the probability of a trajectory ($p(\tau)$) as a markov chain 
 ![[Screenshot 2020-10-27 at 11.45.35.png]]
 To create a markov chain we can augment the space to include actions, you can see the markov chain on (s, a).
 
 Normally using the chain rule of probabilities one should condition the future state on all previous states, but here we are using the markov property to drop the dependency 
 
 $p(s_{t +1}, a_{t + 1}| s_{1:t}, a_{1:t}) = p(s_{t +1}, a_{t + 1}| s_{t}, a_{t})$
 
The objective of RL:
**Maximise the expected value of the sum of rewards over the trajectory**

(Page 13)
Using the augmented markov chain we express the objective 

$\theta* = \arg, \max_{\theta} \sum_{t}^T E_{(s_t, a_t) ~ p_{\theta}(s_t, a_t))}[r(s_t, a_t)]$

Q: *Why?*
To extend the formulation to the infinite horizon cases, where $T = \infty$

(Page 14)
Q: *How do we make the objective finite?*
A non common case is to use the average reward. 

With the assumption of [[Ergodicity]]  and aperiodicy, the stationary distribution (over states and actions) exist $\mu = T \mu$.

(Page 15)
In an infinite episode, there are finite terms of rewards sampled from a non-stationary distribution but assuming that $p(s, a) -> \mu(s, a)$ then there are infinite amount of terms related to the stationary distribution. Therefore 
![[Screenshot 2020-10-27 at 12.03.55.png]]

Q: *Does p converges to a stationary distribution?*
Commont: it must an attractor (eigenvalue though is positive)

**RL is alwasy about optimizing expectations**

(Page 16)
**The reward might be non smooth (+1, 0, -1), but it's expected value can be continous in the parameters of the corresponding distribution**

![[Screenshot 2020-10-27 at 12.07.44.png]]
(bernoully distribution as policy, probability $\theta$ of falling off)

This is why we can use smooth optimizaters like [[SGD]] to optimize for expected value of rewards. 

## Algorithms

(Page 18)
Anatomy (repeated):
- Generate samples: sampling trajectory from your policy (ie running the policy)
- Learn a model/estimate return
- Improve policy

(Page 19)
Policy gradient algorithm

(Page 20)
Model based RL: learn a model NN, $s_{t + 1} = f_{\phi}(s_t, a_t)$

(Page 21)
Generate samples: can be very slow (and expensive) in the real world, while very cheap in the simulator. 

Fit a model: if estimating return is very cheap but if you are learning a model might be very expensive

Improve: if taking a gradient step very cheap, if you have to backprop it might be very expensive 

(Page 22)
## [[value function]]

We can expand the expectation over the trajectory: 
![[Screenshot 2020-10-27 at 12.17.12.png]]

What if we a function that tell us what is inside the expectation, by definition:
![[Screenshot 2020-10-27 at 12.18.01.png]]

Therefore we can modify the objective:
![[Screenshot 2020-10-27 at 12.19.21.png]]

(Page 24)
[[Q-Function]] are also known as action value functions. 

Summary of definitions:
![[Screenshot 2020-10-27 at 12.25.51.png]]

(Page 25)
Q:*How do we use them?*

1. If we know the Q-function we can improve the policy ([[policy iteration]])
2. compute a gradient to increase the probability of a good action
if $Q^{\pi}(s, a) > V^{\pi}(s)$, then a is better than average, so modify $\pi(a|s)$ to increase the probability of a.

(Page 26)
These function evaluate how good your policy is

(Page 27)
## Type of algorithms 
- [[Policy Gradient]]: directly differnetial objective
- [[Value-Based]]: estimate value functions 
- [[Actor-Critic]]: estimate value function of the current policy to improve it
- [[Model-Based RL]]: for planning, improve a policy, else...

## Tradeoff between algorithms 
Tradeoff:
- Sample efficiency: how many samples do you need before getting a good policy?
- Stocastic or deterministic env?
- continous or discrete state?
- episodic or infinite horizon?

(Page 36)
Off-policy vs On-policy (less efficient)

Why would we go for less efficient algorithms? 
For example if the simulation is cheap, on-policy methods can be very advantageous.

(Page 38)
RL is often not pure gradient descent, and convergence is not guaranteed. 
For example in Model-RL there is no guarantee that having a good model will lead to better policies. 

(Page 39)
Assumptions:
- Full observability (access to the state)
- Episodic learning: often assumed by policy gradient methods 
- continuity or smoothless: smoothness of value function

### Examples 
Algorithms:

[[Q-Learning]]
[[DQN]]
[[Value Iteration]]

[[Reinforce]]
[[Natural policy gradient]]
[[Trust region policy optimization]]

[[A3C]]
[[SAC]]

[[Dyna]]
[[Guided policy search]]






