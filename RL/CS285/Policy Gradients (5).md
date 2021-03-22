#cs285 
#lecture 

(Page 4)
You can find the J($\theta$) by rollouts.
An estimated value can be found by averaging over the many different trajectories (sum over samples of $\pi_{\theta}$)

(Page 5)
To imporve the policy we need to estimate the gradient of the policy.

$J(\theta) = E_{\tau ~ p_{\theta}(\tau)}\int p_{\theta}(\tau)r(\tau)d\tau$

by using the definiton of expectation. $p_{\theta}(\tau)$ stands for the [[Markov Chain]] which require knowledge of the initial state distribution, transition probabilities and policy.

The gradient over $\theta$: 

$\nabla_{\theta}J(\theta) = \int \nabla_{\theta} p_{\theta}(\tau) r(\tau) d\tau$

The gradient over the probability distribution of the trajectories cannot be evaluated. 
By using the identity $log(f(x))' = \frac{f(x)'}{f(x)}$ we can rewrite it under the policy expectation. This is nice because we can evaluated expectation under the current policy just by sampling. 

(Page 6)
Derivation.
Initial state distribution and transition probability do not depend on theta and we know how to take a gradient over the policy because we specify it. 


$\nabla_{\theta}J(\theta) = E_{\tau \sim p_{\theta}(\tau)}[(\sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta}(a_t|s_t))(\sum_{t =1}^t r(s_t, a_t))]$

In practice we can generate many trajectories (N) and average over them:
$\nabla_{\theta}J(\theta) = \frac{1}{N}\sum_{i = 0}^N[(\sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t}|s_{i,t}))(\sum_{t =1}^t r(s_{i, t}, a_{i, t}))]$

$\nabla_{\theta}J(\theta) = \frac{1}{N}\sum_{i = 0}^N \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t}|s_{i,t}) Q_{i, t}$

$Q_{i, t} = \sum_{t' = t}^T r(s_{i, t'}, a_{i, t'}))$
and then we can take a step in the policy using [[SGD]]. This algorithm is called [[Reinforce]].

In the rest of the lecture we describe how to implement them in practice. 

(Page 9-10)
Q:*What is the grad of the log of the policy?*
One can intepret reinforce as weighting (by the reward) the maximum likelyhood objective (that is also commonly used in supervised learning).

(Page 11)
Q:*What about continous policies?*
You can have a gaussian policy, with mean specified by a NN. 

(Page 12)
You want to increase the probability of good trajectory and lower the probability of bad stuff. 

(Page 13)
 You can use policy gradient in [[POMDP]]. 
 
 (Page 14)
 Problems with policy gradient:
Bellcurves represent the distribution of the trajectories and the bar represent the reward.
![[Screenshot 2020-10-27 at 18.13.04.png]]
By taking the gradient the blue curve will move to the right and avoid the big negative sample. 

Now consider the case of before but let's add a constant (that does not change the optimal $\theta$)
![[Screenshot 2020-10-27 at 18.16.07.png]]

This are all issues of **high variance** in the gradients. For infinite samples this does not matter but for finite cases it matters. To use it in practive we have **to lower the variance**.

## Reducing Variance 
(Page 17)
*Causality* rewards in the past are indipendent of decision in the present. 
The derivation so far did not make use of causality
 ![[Screenshot 2020-10-27 at 18.26.02.png]]
 we can replace the reward over all the time steps with the actual expected  cumulative reward for the future (cost-to-go) (you can actually prove that the expected value of the gradient for rewards in the past average out to zoro given enough samples). This estimator is still unbiased but lower variance. 
 ![[Screenshot 2020-10-27 at 18.29.16.png]]
 
 (Page 18)
 *Baseline*
 You can center the reward of the trajectories by subtracking the mean reward (not the best baseline):
 
 ![[Screenshot 2020-10-27 at 18.30.32.png]]
 
 You can prove that the estimator is still unbiased by showing that 
 $E{\nabla_{\theta} log p_{\theta}(\tau)b} = 0$
 
 (Page 19)
 Tutorial on how to evaluate the variance of an estimator. We can also derive the optimal baseline to decrease the variance. 
 
 The second term in the Var does not depend on b (unbiased in expectation) thefore for the gradient (to be set to 0 to find optimal b) we only consider the first term.
 
 ## [[Off-policy policy gradient]]
 #offpolicy 
 Every time you modify the policy you have to generate the new samples. We have to throw out data from previous time-steps. 
 
 This is a problem when using deep-rl, neural network just change at tiny bit after a gradient step. 
 
 (Page 23)
 We can use [[importance sampling]],  a techique to evaluate expectation under a distribution given samples from another distribution. 
 We need to compute the ratio of the trajectory distribution, ie the ration of the products of the policies. So we can evaluate them. 
 
 (Page 24)
 If we want to estiamate new parameters $\theta'$:
 $J(\theta') = E_{\tau \sim p_{\theta'}}[r(\tau)] = E_{\tau \sim p_{\theta}}[\frac{p(\theta')}{p(\theta)}r(\tau)]$
 
 Now we can compute its gradient
 ![[Screenshot 2020-10-27 at 19.20.41.png]]
 
 (Page 25)
 If you ignore the importance weights on the reward you recover [[policy iteration]]. 
 
 (Page 26)
 The first terms of weights is exponential in T, if the weights are less than 1 then the product will go to zero exponentially fast. This means that the variance will be come infinitely high. 
 
 Now the trick is to use marginal probabilities $\pi_{\theta}(s_{i, t}, a_{i, t})$ instead of conditional prob $\pi_{\theta}(s_{i,t}|a_{i,t})$. 
 
 Q: *Why?*
 Sampling state action pairs from the state action marginal 
 $(s_{i, t}, a_{i, t} ~ \pi_{\theta}(s_t, a_t))$ is equivalent. 
 Now you can use the ratio of the marginal probabilities for importance sampling, this itself is not very useful since we don't know the initial state distribution and transition probabilities. 
 
 We ignore the state marginal (why?) and we get a similiar equation to the one above expect only the ration at time t is considered. 
 

 
 ## Implementing policy gradients 
 
 We would like to make use of automatic differentiation. 
 
 The pseudo loss, **is not** the RL objective, it's a quantity that is convenient to obtain the right gradient by making use of automatic differentiation.
 
 (Page 29)
 N = num samples
 T = num timesteps
 Da = number of action
 Ds = number of states 
 
(Page 31)
This is not supervised learning! 
Batches of 1000-10000 are common to reduce variance 
Finding the learning rate is hard 
More hyperparameter tuning 
 
 ## Advanced Policy Gradients 
 (Page 34)
 1 Dimensional state space, goal is to reach the state 0. 
![[Screenshot 2020-10-28 at 08.51.30.png]]
penalty for large action and being far from zero. 

Actions are continous, a gaussian. 

You can visualize the gradient as a vector field. 

This is equivalent to the problem of poor conditioning, if you are using [[SGD]] on a quadratic function with very different scales for the eigenvalues you'll have a hard time. 

(Page 35)
Choosing a single learning rate is hard. You want large lerning rates for parameters that don't change a lot the policy. 

Gradient ascent can be seen as solving a constraint optimization problem: withing a ball of your parameters, find the parameter that maximizes the linearization of your objective ($\alpha$ can be seen as the largrangian multiplier).

We want to reparametrize the space from parameter space to policy space. 
We choice a divergence matrix (KL), and we take up to it's second order expation (a quadratic form), to ensure policy don't change too much (ball in the previous examples).

(Page 36)
So we get our natural gradient:
![[Screenshot 2020-10-28 at 09.01.47.png]]
F is basically our preconditioner


 
 
 