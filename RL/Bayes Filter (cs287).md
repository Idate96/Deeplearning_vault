#lecture 
#cs287 

Page 13
Derivation of [[Bayes Rule]]. 

Imagine x is the state of the system and y is a sensor reading. So we want to find a probability distribution of the states given a sensor reading. 

$P(y|x)$ is a causal model, this is the sensor model (like precision at 2 m is +- cm from LIDAR)

$P(x)$ is a prior over the states

$P(y)$ serves as a normalization 

We often write:
$P(x|y) = \frac{1}{Z} P(y|x)P(x)$

---
Law of total probability with conditioning over a variable z:

$P(x|z) = \sum_y P(y|z)P(x|y, z)$

just add the conditioning to all the probability distribution. This formula will be used very often because observe things in the past and we want to condition the probability distribution on those observations. 

With conditioning you are basically restricting the available world, putting all probability on the marked rectangle and renormalize it. 
[[Screenshot 2020-11-21 at 15.05.17.png]]

[[Bayes Rule]] can also be used with conditioning. 

---
Page 17
[[Conditional Indipendence]]

---
Page 19
What is available to us, the causal model, is not what we want. 


## Bayes Filter 

We can build a causal model $p(x'|u, x)$ to take into account the effect of actions. 

$p(x'|u) = \int p(x'|u,x)p(x) dx$

 Given the action u and the  probability distribribution of the currrent state we can find the conditional probability distribution of the next state. 
 
 Measurements use bayes use, becuase we have the causal model. 
 
---
Page 35
Measurement at time t, depends only what happens at t. If your state space does not capture everything about the world this assumption is invalidated. 
 

 ---
 Page 36
 
 Definition of Belief:
 $Bel(x_t) = p(x_t|u_1, z_1, ..., u_t, z_t)$
 
 We can derive
 
 $Bel(x_t) = \eta p(z_t|x_t) \int p(x_t|u_t, x_{t -1}) Bel(x_{t-1}) dx$
 
 ---
 Page 37
 Algorithm 
 
 ## Gaussians
 [[Gaussians]]