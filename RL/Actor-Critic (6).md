#cs285 
#lecture 

(Page 3)
Can we get a better [[action-value]] function?
In [[Reinforce]] we use a single sample for the reward to go. There is randomness in the system, in the [[policy]] and in the [[MDP]]. An this problem is related to the high variance of the [[Policy Gradients (5)]].

![[Screenshot 2020-10-28 at 09.13.10.png]]

(Page 4)
We can still use a baseline as we did for the reward.

But it's actually better to compute the expected reward (not in the time-step, ie $Q(s_t, a_t)$) in that state, ie $V(s_t)$. So a very good choise for the baseline is the value function. 

We can define the [[advantage function]] as the difference between the action-value and the value function. 

(Page 6)
Q: *Fit what to what?*
$Q^{\pi}(s_t, a_t) = r(s_t, a_t) + \sum_{t' = t}^T E_{\pi_{\theta}}[r(s_t', a_t')|s_t, a_t] = r(s_t, a_t) + E_{s_{t + 1} ~ p(s_{t + 1}|s_t, a_t)}[V^{\pi}(s_{t+1})]$
Now we make an approximation and we use the value function at the next *observed* state to approximate the expectation (single sample estimator, i.e. we substitue for the value of the state $t + 1$ that we observe instead of taking the expectation):

$Q^{\pi}(s_t, a_t) \approx r(s_t, a_t) + V^{\pi}(s_{t+1})$

Now the [[advantage function]]:
$A^{\pi} \approx r(s_t, a_t) + V^{\pi}(s_{t + 1}) - V^{\pi}(s_t)$

*We should probably fit the value function*

(Page 7)
The step of fitting is also called policy evaluation.  

We can use a [[Monte Carlo]] estimate like in policy gradient. Run the policy many times and sum the obtained rewards during the episode.  

$V^{\pi}(s_t) = \frac{1}{N}\sum_{i = 1}^N \sum_{t' = t}^T r(s_t', a_t')$ is usually not possible becuase it requires the simulation to be started over again at state $s_t$, while usually we can only do for the initial state.

(Page 8)
Fitting a NN actually reduces the variance of the estimator (generalization). 
Two similiar states should have similiar V, values

For each state on the rollout, we assign as label the estimated cost to go. We then train the network to minimize the square error. 

In the end the label target for the network has become
$y_{i, t}  = \sum_{t = t'}^T E_{\pi_{\theta}}(r(s_{t'}, a_{t'})) \approx r(s_{i, t}, a_{i, t}) + V^{\pi}(s_{i, t + 1}) \approx r(s_{i, t}, a_{i, t}) + V^{\pi}_{\phi}(s_{i, t +1})$

(Page 9)
But we can also use our "recursive formula" the action-value function, the label this time is the reward plus the expected value function at the next observed state ([[bootstrais the previous value function (NN) pping]]).

$V^{\pi}_{\phi}(s_i, t+1)$ is the previous value function (NN) 

## From evaluation to actor critic
(Page 12)
step 2 was the previous part of the lecture. 
![[actor-critic.png]]

### Discount Factor 
In infinite horizon setting, every time we use a bootstrapping rule the value function will increase in value, possibly going to infinity.

Trick: *We want rewards sooner rather than later*

$y_{i, t} = r(s_{i, t}, a_{i, t}) + \gamma V^{\pi}_{\phi}(s_{i, t + 1})$

This is equivalent on modifying the [[MDP]] by adding a death state.  

(Page 14)
Two ways of adding discount to [[Monte Carlo]]. 

Options:
1. Only rewards are discounted
2. Because of the discount we care less about rewards *and* decision. that's why the $\gamma$ factor is in front of both rewards and policy 

Q:*If we apply causality in option2 do you get option 1?* NO!

Option 2 does not match the critic. Because you have this discount you care less about both reward and decision (by discounting the gradient). Option 2 is actually the correct formulation, decision in the future matter less. 

(Page 15)
In reality this is often not what we want. Often we use option 1! 
We want the RL agent to do tasks as running as far as possible and option 1 help us in making the reward finite. Maybe what we really want is average reward, but unfortunately is kind of hard to compute. 

Discounts also reduces the variance (at cost of bias)

(Page 16)
So far we used actor critic in an episodic setting where we update the agent after a batch of episodes are finihsed. 

But we can also use it online, at every single time-step. 
There are few problem with this formualation on deep-RL

(Page 18)
## Implementation Decision
Simple way to start is to have two seperate network, one for the policy and one for the value function. 
Disadvantage: no sharing of features that could be helpful for both.

So we can also have a shared architecture where the outputs are both the value function and the policy, and the shared layers can be the convolutional layers. This descreases the stability though, because the NN receive both gradients. 

(Page 19)
Batch size in online fashion is 1. Maybe we can use multiple steps (such as 10-20), but this is also not very good, because the steps are very correlated. A better choice is to use **parallel workers**. 
Now the samples of the mini batch is less correlated, because the workers are decorrelated. 

If you have a very large pool of workers you should use asyncronous parallel actor-critic. The parameters are updated at each time step and send back to the workers. 

A potential issue, is that the samples that are used to take an action are not the lastest ones (present in the central server), therefore the update rule might be invalid, because we are working on policy (we would need to use importance sampling otherwise). Usully it still works because the differences are small. 

## Critics as Baseline
(Page 21)
The advantage of the actor-critic is to drastically reduce the variance, but no longer unbiased (compared to policy gradients where rewards are just summed).

We can use a state dependent baseline, a good choise is the value function. 

(Page 22)
[[Control Variates]]
You can also use a baseline dependent on both the state and the action. 

The equation in the last line is what you should use if you want to have the advantage function defined in the line above. 

## Eligibility traces & n-step 
[[Eligibility Traces]]

On one side we have a TD(0)(low variance, some bias) estimate and a MC estimate (high variance, no bias).

N-step return estimator can give us something in betwee. You sum your rewards until t + n, you subtrack the baseline and add the n-step ahead value function. First term control the variance. 

(Page 24)
[[GAE Estimator]]
Weighted average off all possible n-step estiamator. You prefer cutting earlier to have lower variance. Tipical choice is to use an exponential fall-off, which give a nice way to evaluate the expression. 

## Examples 












