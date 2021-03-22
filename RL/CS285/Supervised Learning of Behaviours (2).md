#lecture 
Page 3:
There is a big different between state and observation. In the case of hunting situation, the image the cheetaa is seeing is the observation while the physical position and velocity of the two animals is the state. 

$\pi(a_t|o_t)$ - policy 
$\pi(a_t|s_t)$ special case of the policy 

The *observation might not enough to deduce the state*

[[Graphical models]] can show the difference between observation and states![[Screenshot 2020-10-26 at 21.05.21.png]]

If you want to make a decision is enough to know your current state (Indipendence of Markov property) and forget about the previous state. 

In this case the policy is **conditioned** on observations.
Q: *Are observation  conditionally indipendent in the same way? *
They usually don't satisfy the Markov property. Past observation can give you usefull information. 

## Behavioural cloning 
*page 7*

Learn a policy by copying the steering commands, given an image, of humans. 
Q: *Does it actually work?*
In general **No**

Q: *Why*?
Policy slights deviate from training trajectory (small mistake), now it ends up on unfamiliary states (out-of-distribution) and makes even bigger mistakes. So the mistakes compound on each other 

Nontheless in practice they work relatively well given enough data.

Q: *Why do they work in practice?*
(Page 9)
You add 3 camera, teh two side cameras are trained to correct for small mistakes. You can add to the training data, data about how to correct for this mistakes. 

(Page 11)
We would like to that our observation distribution is the same as the training distribution. In practice they different because the policy is different. 
A solution is to make the policy perfect.

(Page 12)
Q: *Can we be more clever about the way we collect data?*
Dagger! it solves the problem of distributional shift

**Problem with Dagger**: asking humans for optimal actions is very difficult and impractical. 

(Page 17)
Human behaviour might be non Markovian, if see the same thing twice we behave in the same way, but usually people don't. Humans are not very consistent.

(Page 18)
A common solution is to use a [[RNN]]. In the slides the images are processed by a [[CNN]]. 

(Page 19)
**Non Markovian behaviour.**
*Q: why it might not work so well?*
For NN is not possible to establish causation (pedestrian and light goes on when braking). 

(Page 20)
**Multimodal behaviour**
In case of discete actions this is not a problem. 
![[Screenshot 2020-10-26 at 21.29.04.png]]

But in a continous action setting we often parametrize the action value as a gaussian, so if we have to go left OR right we might actually end up in the middle (average).

*Solutions*:
	- [[Mixture of Gaussian]]: more output parameters and higher the dimensionality the harder it is (like controlling all the joints of a robot)
	![[Screenshot 2020-10-26 at 21.31.57.png]]
	- [[Latent Variable Models]]: takes in an image and **noise**. Examples of these systems are [[Conditional Variational Autoencoders]] and [[Normalizing Flows]]. These models have been shown to be able to approximate any distribution.
	- [[Autoregressive Discretization]]: nice middle ground but easier to use. It discretizes one dimension of the action at the time (no exponential cost). A NN takes an images and outputs the discretization of dim 1 of *a*, then we sample from the softmax and we get a value for the action.  Then we feed this value to a NN that samples the action over the dimension number 2.

(Page 29-onward)
**Some issues with imitation learning**

(Page 34)
*What is a good cost function for imitation learning?*

$r(s, a) = log p(a = \pi^{*}(s)|s)$
where $\pi^{*}$ is the policy of the expert. 
**The reward must be evaluated in expectation under the learned policy not under the expert policy. Which means we would like to take the *expert* actions in the states we visit and not the ones that the expert visited.**

So behaviour cloning is not usually optimizing for the right problem (which Dagger tries to correct).

(Page 35)
Assume supervised learning work: for all states in the training set the probability of making a mistake is $\epsilon$.

Cost is 1 if mistake is made, 0 otherwise.

Let's try to bound the expected cost of rope walking agent: 
 ![[Screenshot 2020-10-27 at 09.55.17.png]]
 
 This is a bad bound, since the error increases quadratically for the length of the episode. 
 
 (Page 36)
 A more general analysis takes into account generalization (we don't need $s \in D_{train}|$).
 
 It's enough to say that the deep learning works for states stampled from the same distribution as the training one:
 $\pi(a \neq pi^{*}(s)|s)}) < \epsilon$
 for $s ~ p_{train}(s)#$
 
 With Dagger (all point of dagger) $p_{train} -> p_{\theta}$ so 
 $E(\sum_t c(s_t, a_t)) < \epsilon T$
 
 if $p_{train} \neq p_{\theta}$ :
 ![[Screenshot 2020-10-27 at 10.01.34.png]]
 we use $p_{mistake}$ because if we make an error we don't know if we are in distribution or not.
 
 Now we can bound the total variation diverenge, sum of absolute values of probabilities over all the states (we will use this identity for the next line):
 ![[Screenshot 2020-10-27 at 10.04.16.png]]
 if we don't know anything aobut $p_{mistake}$ than we have to put a bound of 2 for that. 
 
 Now the expected value of the cost. By adding and subtracting $\p_{train}$ to $p_{\theta}$ we can use the above identity to find that the cost is still $O(\epsilon T^2)$.
 
 ## Another way to imitate 
 
Data that is not necessarility optimal for one task could be optimal for another. 

(Page 39)
What if you have demostrations for many differend end point of the trajectory p1, p2, p3. In fact you may not have enough data for each specific point (say to reach p1), but what if we condition the policy on the end point: $\pi(a|s,p)$. This policy than can reach any point. This can be done by *appending p to the state*, say in the input of a NN. 

(Page 44)
No human data at all. Random policy is the start policy, the end state of the random policy is relabled and the policy improved with that. 
*Learning to Reach goals via iterated supervised learning*


 
 
 
 
 
 