#cs285 
#lecture 

**We focus on how to learn models**

---
*Page3*
Most obvious algorithm. 
Run a base policy and record the transitions. Then you can use the transition to train a model. Now you can use the methods of [[Optimal Control and Planning]] for planning. 

---
*Page4*
The base policy must explore the different modes of the system, so you need a good coverage. 

---
*Page5*
Unfortunately this method does not really work with high capacity models. 

Example: Suppose you want to get to the top of the mountain. 
So first random walk on the mountain and then use the gather data to learn the dynamics. We ask f(x, a) to predict how high I will get. 
The model thinks going to the right makes you go higher, until you fall!

Q:*Why a model trained on the base policy can lead to bad outcomes*?

The data used to train the model comes from states induced by the policy $\pi_0$. 
The issue is that, when you plan you execute plan  $\pi_f(s_t)$ (planning algo run on to of the model $f$), with its own state distribution $p_{\pi_f(s_t))}$. This distribution evoles to very right and down the mountain. 

The problem is that $p_{\pi_f}(s_t) \neq p_{\pi_0}(s_t)$, we are experiencing [[distributional shift]].
This distributional shift happens becuase of the dynamics model while on behavioural cloning happened because of the policy. 

When we plan with the model, we choose states with high reward, we end in states with low probability under $\pi_0(s_t)$, this will cause the model to make erroneous predictions at those states. The model then makes erroneous predictions at the (next) erroneously predicted next states, making things even worse. 

This happened before in [[Supervised Learning of Behaviours (2)]]. 

If we a low dimensional state space it's very hard to overfit the training data (say three coefficients of an aircraft model).

When we use NN this distributional shift is a big problem because more expressive models will overfit more the scene that we are in. In system identification the number of parameters instead is very limited. 

---
*Page6*
Q:*Can we make the [[state distribution]] of one policy equal to another ?*

With [[Dagger]] we a human expert telling us what the optimal action is, here is much easier because you can take the action and actually observe the next state.  

Now we can obtain the simplest model base RL algorithm. 
We just add step 4 to the previous algorithm, where now we append the data we observed to the dataset to train the model. 
 
---
*Page7*

Q:*what if you make a mistake?*

Say that your model predict that the car will go strainght when the steering wheel is a bit to the left. When you execute the model you will go to the left, more and more, when you add the data to the training set this error should disappear.

But we can do better if we fix immediately the mistake. 

---
*Page8*
This is basically [[MP[This is basicallyC]]. 

Execute only the first planned action and observe the state in which you get into. add the data to the dataset. Then replan for the next action. 
Aften N step you can retrain the model.

---
*Page9*
Q:*How do we replan?*
THe more you replan the less accurate the plan needs to be. A common this is to make the control horizon shorter (compared ot the open loop). 

---
## Uncertainty 
*Page11*
Example with algorithm 1.5.

Basic idea it to train an model base agent first (until reward is 500 in this case), the use the current policy as initial policy for a model free agent which reaches a reward of 5000. 
In blue you see the model free learner without the initial good policy. 

---
*Page12*
Q:*Why the model based learner is so much worse than the model-free learner?*
In model based RL the model must be already pretty good at the beginning to provide effetive exploration, while at the same time not overfitting. NN are pretty good at large data regimes but not so much with little data. This causes the agent to get stuck. 

---
*Page13*
Distributional shift and overfitting are the causes. 
If the model overfitts it will have a lot of spurious peaks that the planner can exploit (mistakes). So it's worse than the regular overfitting problem.
![[Screenshot 2020-11-03 at 18.20.48.png]]


---
*Page14*
Uncertainty estimation can mitigate the problem. 
Instead of predicting the next state you can predict the distribution of the next state given a certain uncertainty about the model. 

Say you want to walk on the edge of the cliff. If you are very confident you might go directly on the edge but if you are uncertain you might stay a bit back, which still yields the highest expected rewards (it's a bit like SARSA and Expected SARSA}).
*It forces the planner to take actions that are good in expectations.*

---
*Page15*
Take actions with high reward in expectation

---
*Page16*
- You need to still explore, if you are too conservative you might not explore at all. 
- This is not an algorithm that tries to be robust

## Uncertainty aware models

---
*Page 17*
Q:*How can we train uncertainty-aware models?*
Idea 1: *Entropy of the output of the NN*?

The probability distribution can be generate with a softmax or a multivariate gaussian distribution. 

Q:*Why not enough?*
The planner will try to exploit the model selecting out of distribution action that lead to high reward, this leads us in out of distribution states and it continous on... 
If the model is outputing the uncertainty and trained with log-likelihood our uncertainty for out of distribution states will also be inaccurate (erroneuos mean and variance). 
**The uncertainty of the NN output is the wrong uncertainty**

Immagine a highly overfitted model, it's gonna output a mean that is equal to the actual value and a variance equal to zoro. This model is still very wrong. These model will be both incorrect and overconfident on test points. 

Two very different types of  uncertainty: 
- aleatoric or statistical uncertainty (data is noisy)
- model uncertainty (don't know the model)

If the true function is noisy aleatoric uncertainty does not go down by collecting more data. 
Say the model that predict rolls of dice, the entropy of the model will be constant with more data. 

---
*Page19*
Estimate model uncertainty, which in case of parametrized network, is being uncertain about the weights. 

**Beign uncertain about the model is being uncertain about $\theta$. 

With a uniform prior:
$arg,\max_{\theta} \log p(\theta| D) = arg, \max_{\theta} \log p(D|\theta)$
on the right hand side is the maximum likelyhood estimator. 
But can we estimate the full distribution $p(\theta|D)$ instead of just getting the most likely model.  The entropy of this distribution will tell us about model uncertainty. 

The posterior distribution over the next state is:
$p(s_{t + 1}| s_t, a_t) = \int p(s_{t + 1}| s_t, a_t, \theta) p(\theta|D) d \theta$
by averaging out the parameters. This integral is clearly intractable in the case of a NN. 

---
*Page20*
[[Bayesian Neural Network]]
A common approximation is to say that the weights distribution are indipendent from each other:
$p(\theta|D) = \prod_i p(\theta_i|D)$
This is not a very good approximation but makes things easy.
A common choice is the gaussian distribution. 

---
*Page 21*
[[Network ensembles]]
This a simpler method that actually works better for model-based RL. 
Idea: instead of training a neural network, we train many different NN and diversify them. So that they learn a slightly different model and their error indipendent from each other. Then they could vote about the predition and we could estimate uncertainty from that. 

This is equivalent by averaging delta functions for the posterior (where each delta function is a NN). 
In continous spaces this does not mean that we average the mean state but rather the probabilities. If each of this model is gaussian (common output for continous action NN), the average is a mixture of gaussians. 

Each model should be trained on indipendent data that still comes from the same distribution. We can choop our training set in many pieces. A cool trick is to sample the data from the dataset with replacement (so we keep the data in).

---
*Page22*
Practice: 
It appears that resampling with replacement is not necessary thanks to [[SGD]].

---
## Planning with uncertainty
*Page 24*
Now we have N models and we want to maximize the **aerage rewards of the models (deterministic)**.

Step 1: choose one of your model (if you have an ensable)
Step 2: predict the next state with your model 
Step 3: calculate the reward
Step 4: repeat to get an everage reward 

To select the actual action sequence you can use your optimizer of choice (for example a [[Cross Entropy Method]])

---
*Page 25*
Q:*Does it work*?
They work really really well! 

---
*Page 26*
[[PDStep 1: choose one of your model (if you have an ensable)
DM]]

---
## Model based RL with Images

*Page29*
We usually deal with [[POMDP]]
We don't even know what is the state if we have just images. 

- High dimensionality: many pixels
- Redundancy: many things have the same color, irrelevant objects
- Partial obervability: from one frame you cannot decude speed of an object 

**Nice separation of work**
Idea: we can learn seperately learn $p(o_t|s_t)$ (high dimensional but not dynamic) and $p(s_{t + 1}| s_t, a_t)$ (low dimensionla but dynamic)

(why p(o|s) and not p(s|o))?

Q: *What is an observation model ?*
How a state maps into an image. 

---
*Page30*
*State space/ [[lantent models]]*

We are gonna learn the observation model: how a state maps to an image
The equation for the latent space is not a derivation is just max likelyhood applied to the dynamics and observation model. 

With a latent (state space) model we could apply maximum likelyhood if we knew the state but since we don't know it we have to use the expectation (*that's where you get the state from*)
$max_{\phi} N^{-1}\sum_i^N \sum_t^T E[\log p_{\phi}(s_{t + 1, i}|s_{t, i}, a_{t, i}) + \log p_{\phi}(o_{t, i}|s_{t, i})]$

The expectation is drawn over:
$(s_t, s_{t + 1}) ~ p(s_t, s_{t + 1}|o_{1:T}, a_{1:T})$
You need a model (posterior) that is able to estimate the state from a sequence of images and actions.
**The posterior is used to sample the states of the system**

---
*Page31*
Q: *How do we do it?*
Learn an approximate posterior $q_{\psi}(s_t|o_{1:T}, a_{1:T})$ that is a NN, that extracts the current state from a sequence of images and actions. This is the *encoder*.

There are many posterior options:
- $q_{\psi}(s_{t + 1}, s_t|o_{1:t}, a_{1:t})$: full smoothing posterior that gives you exactly the quantities that you want.
- $q_{\psi}(s_t| o_t)$, predict state from the current observation. Easiest position to train but also the furthest from the actual posterior. This is called "single-step encoder". Use only if the partial observability effects are not too strong.

If you have a heavily partially observed setting you should aim towards the full posterior while if the contrary is true the single step encoder might be enough. 

Q:*How do you train these posteriors?*
[[Variational Inference & Generative Models]]

---
*Page32*
Special case is to use a deterministic encoder. A simple neural network should fit this cathegory. 
We can substitute the encoder everywhere we see a state (no sampling).


---
*Page39*
What if don't learn an embeding (mapping from observation to states) and learn an observational posterior (predict next image)?

You need a recurrent model to make use of previous images. 