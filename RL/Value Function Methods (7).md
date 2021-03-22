#cs285 
#lecture 

(Page 3)
Maybe we don't need an explicit policy. 
The [[advantage function]] tells us how much better is $a_t$ compred to the average action. So the arg max is the best action, that $\pi$ could have chosen, **regardless** of what $\pi$ is (like random policy).

## [[policy iteration]]
(Page 4)
How do you evaluate the advantage $A^{\pi}(s, a)$. 
How do we evaluate V(s)?

(Page 5)
We can use [[Dynamic Programming]] to estimate $V^{\pi}(s)$. 

We can compute exaclty the bootstraped update, since the expected values can be computed with the transition probabilities. 
 In case of a derministic policy we don't need to take expectations over actions,
 
 (Page 6)
 You can show that iteration converges to a fixed point. 
 
 ## Fitted Value Iteraton
 
(Page 7)
 the arg max of the advantage function is the same as the argmax of the [[Q-Function]]. 
Key idea: **Skip the policy and take the max of the q-function to evaluate the value function**
This algorithms is called [[Value Iteration]]. 

(Page 9)
Let's talk about how we can introduce function approximation and NN.
We approximate the value function, which is fitted to the max of the q function (computed on the go).

(Page 10)
**Modification to not need transition dynamics**

The fitted value iteration still requires the transition dynamics, the evaluation of the expectation and in the fact that it requires us to simulate multiple dynamics:
![[Screenshot 2020-10-28 at 19.18.55.png]]
(unless we have teloporting the only way to do this is to know the transition dynamics)

*What if we construct a Q-function recurrence in an analogs way?*

Since the Q function is a function also of the action, we don't need to evaluate different actions (since we don't have time travel, we need the transition probabilities, or a model of them) to discover the maximum of $r(s_i, a_i) + \gamma E[V_{\phi} (s'_i)]$. We can just do a roll-out of the policy:
![[Screenshot 2020-10-28 at 19.51.58.png]]

This simple moditification allows us to do policy evaluation without the transition dynamics. if we have $\pi(s)$ in the distribution to get the next sample then we necessarily need an explit formulation of the transition probability. By replacing the policy output for the actual action taken now we can approximate the expected value with samples. To do this need to approximate the Q function 
![[policy_eval.png]]

Now as the policy $\pi$ changes the action, a, does not actually change, which means if I have samples I can use them to fit the Q-function. The policy shows up in the argument of the Q-function at the state s', inside the expectation.

(Page 11)
We turned policy iteration into value iteration using the "max trick".

Can we do the same with Q-values?

The trick in fitted Q iteration is that in step one is to replace the expectation of $V_{\phi}(s_i')$ with $max_a Q_{\phi}(s_i', a_i')$, crucial is that instead of taking the max over all possible states, we only take the max over the state that the agent has ended up with the current policy. 

It works also for off-policy (unlike Sarsa which is an on policy algorithm).
Then train a NN to output Q values. 

(Page 12)
Step 1, usually the policy is the last policy 

For discrete action is common to output a value for each action.

Step 3, in step you have to choose the nubmer of gradients (or full convergence)

Doing step 3 once does not get you the best Q function, you can iterate step 2 and step 3 (K times) before collection more data (step 1). 

## From Q-iteration to Q-Learning

(Page 14)
What it means for q iteration to be off-policy:

the only place the policy shows up is as argument to the Q-function : 
$Q_{\phi}(s_i', \argmax Q_{\phi})$, 

the arg max is our policy. When our policy changes the maximum changes. But we don't need to generate new roll-outs.

Q function allows to "simulate" the value of new action. 

Given a state and action the transition is indipendent of the policy. 

You have a bunch of transition and you can use tehm to fit the q-value, you care just that the cover enough of the space. 

(Page 15)
In the tabular case you directly write the $y_i$ (or estimated Q) that leads to the maximum action-value, instead here in step 3 we fit a NN to the output.

You are minimizing the Bellman error and if the error is zero the policy is optimal

(Page 16)
The online algorithm, you take one action and observe a transition. You then compute the target value. You take 1 gradient descent on the error between the target value and your q value.


(Page 18)
The final policy is a greedy policy but while learning might not be the best choice as it limits exploration early in the learning. 
The arg max policy is deterministic, and Q at the beginning is random (or arbitrary), it will commit the policy to take the same action for ever. 

One common choise is to use a $\epsilon$-greedy policy to introduce randomness. Usally you vary the value of $\epsilon$ during traning. 
Another option is to take action in proportion to the exponent (or positive transformation of the Q function), in this way almost equally good action will be taken in almost the same proportion and bad ones very rarely. 
This exploration rule could be preferred since you give more chance to the second best action and if you learn that an action is very bad you stop taking it. 
(Boltzan exploration).

## Theory of value based methods
We can define the bellmann operator $B$. The matrix T is taking expectation basically. The fixed point of B is the optimal value function $V^{*}$. 

(Page 22)
We can prove value iteration converges because B is a contraction: it brings the vectors closer. 
Each time you apply B to V you get closer to the optimum $B^{*}$. The norm is the infinity norm.  

(Page 23)
In fitted iteration you take the argmin wrt to $\phi$. $\Omega$ is the set of all possible NN with that architecture. Step 2 a projection in the L2 norm, of BV onto the set $\Omega$ (V'). 

(Page 24)
The operators are contraction for different norms, their combination is not a contraction of any norm. 
Bellmann operator get's you closer to the start but projecting onto $\Omega$ actually gets you further from the star (V' further than V).

(Page 26)
Q-Learning is not taking gradients on a well defined objective. 









