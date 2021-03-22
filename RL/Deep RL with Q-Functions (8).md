#cs285 
#lecture 
 
 (Page 3)
 We obtain this by only taking a single gradient. 
 Step 3 is not a taking a proper gradient, as $Q_{\phi}$ is showing up in mulitple places while there is only one $\nabla Q_{\phi}$. 
  ![[q_learning_gradient.png]]
 *By properly applying the chain rule we obtain the proper gradient but the resulting algorithm have very poor numerical properties.*
 
(Page 4)
Online Q-Learning has problems with time correllated sequencial samples. It tends to overfit each window of time, a possible solution is to parallellize. 

(Page 5)
 Fairly old idea introduced in the 90s. 
 
 ##  Solving moving target problem in Q-learning
 This problem is generally solved with [[Target Networks]]
 
 (Page 6)
 B stands for the [[replay buffer]], samples are taken (iid) from the buffer. In this way the samples (s,a, s', r) are not correlated in time. 
 Where do we get the buffer? 
 You want to feed the buffer with better data wiht the lastest policy. 
 
 (Page 9)
 In Q-Learning we are faced with a moving buffer and it's still not gradient descent
 
 (Page 10)
 We often use one gradient because you don't want convergence under the wrong target (because the target change and the gradient does not take into account the change).
 
 (Page 11)
 Step 4: the policy used as argument of $Q_{\phi'}$ does not change in the inner loop. 
 K 1-4
 N = 10000 because it's very hard to try to fit a moving target (underilined) with supervised learning. After that we update the target with the new policy. 

 
 (Page 12)
Here we show the classic [[DQN]] algorithm.


 $Q_{\phi}$ might be used in the data collection (step 1 classic) if a greedy policy is used. 
 This is a special procedure of the algorithm shown at the top of the slide. 
 
 (Page 13)
 A bit strange to update the target network as before. The step just after the transion experience the most the "moving-target" effect. 

A common choise is (Polyak averaging), linear interpolation of non linear function approximator. It makes sense only if $\phi'$ is close to $\phi$.

## General view of Q-Learning
We covered many algorithms.  
(Page 16)
These processes are always running. When buffer is full you should through out (queue structure) some data. 
Replay buffer is very large (like 1M), and step 1 and step 3 are consequently quite decoupled if you sample from the buffer.
By using different speed we mitigate the non-stationarity effect and help convergence in step 3.

## Improving Q-Learning

Q: *Are Q-values accurate?*

(Page 20)
The actual value of Q-fucntion is not really representative of the actual rewards. 
You can compare $Q_(s_1, a_1)$ with the discounted cumulative rewards obtained during the episode. 

Q-Function estimates are usually much much larger than the actual true value, this is not a fluke. 

(Page 21)
Reason for **overestimation**. 
The max is really the problem. 

$E[max(X_1, X_2)] >= max(E[X_1], E[X_2])$ and this is true because you are often selecting the variable that has higher noise. 

Imaginve the Q-Function is the true Q-function plus some noise, by taking the max you overestimate the next value. 

The idea is to decouple the noise from the action selection mechanism from the value estimation step. If we use the same network for both the noise will be in the "same direction" for both steps.

(Page 22)
We can use two Q-functions, decorellated with each other. 
[[Double Q-Learning]]

(Page 23)
In practice we can use $\Phi$, to select the action instead of  and $\Phi'$ (used to compute the target). Unfortunately they are not really decorrelated the two networks.

(Page 24)
[[TD(N)]] for Q-Learning:

**Early in the training almost all the learnign comes from rewards and max Q term is basically adding noise. Later on in training the Q-value will dominate.**

This gives you higher variance but lower biased. 

(Page 25)
N-step returns don't work with off-policy data since you might take different actions in the future. 

Q: *How do we make this algo off-policy?*

## Continous action
Problem: **How do you take the max?**

(Page 27)
How do you take the max over continous actions?
You want this to be very fast (since it's in the inner loop of training). 

(Page 28)
Just sample different actions and choose the one/compute the maximum of the q-function.

[[Cross Entropy Method]] consist in sampling the actions and reshapes the action distributions (more about it in the future).

(Page 29)
Another choise is to choose a function that is quardratic (easy to optimize). NAF architecture, has a NN output the value, P, and $\mu$. 

The maximum can be found analytically. 

(Page 30-31)
Learn an approximate maximizer [[DDPG]]. 
Idea: train another network to the take the argmax.

(Page 33)
Start off by testing your algorithm on a simple problem where a correct implementation should work. 

Replay buffer on the north of 1M are better to train. 

Reduce epsilon as you go along the training. 

## Practical Advice 
Test on easy reliable tasks.
You have to do three steps:
- Debugging (preferrably on an easy problem)
- Hyperparmeter search
- Solve your actual problem 

(Page 34)
If you have a bad action (say -1000) and some good actions (10, 5, 1) the least square objective will take a **huge** gradient step to minimize the chance of doing a bad action. You **need** to clip gradients or use a huber loss. 

Run multiple random seeds (you see it in the plots too).

(Page 35)



