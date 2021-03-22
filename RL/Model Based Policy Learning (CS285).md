#lecture 
#cs285 

*Page3*
The planner, even though replans at every time-step, still chooses the best set of actions as if they were executed on open loop. It can't plan to make different decision in the future based on new revealed information.

This is foundamentally less flexible than policy learning.
For example consider the math test example: if you ahve two time step you agree or not to take the math test, if you agree you have to give the right answer at the next time step. The problem is that you haven't seen the question so if you commit to take the test you'll probably get the wrong asnwer. As a result the system will end up never taking the test. 


---
*Page4*
We would like a closed loop setting, so you wnat to send a policy not a set of actions. In this way we recover the tradional RL objective. 

*The distribution is only depended only on the initial state while before the objective was conditioned on the all sequence of actions.*

Q:*what policy?*
NN is a global policy but you can also have a time-varying linear policy (local). 

---
*Page5*
Assume everything is deterministic. 
You can see the compution graph (no expected values due to determinisms).  
To maximize the reward we can compute the derivative of the total reward wrt the policy parameters and do backprop on the policy. 

This procedure is also valid for stocastic policies 

---
*Page6-8*
The trouble is that the action at the first time step would have a large effect on the result and therefore get very large gradients (similiar to [[Shooting methods]]). 
This problem is ill condioned (very large and very small eigenvlaues of the Hessian). 

No longer valid [[LQR (CS287)]] procedure becuase now you policy is coupled and going backwards in time is not possible. 


---
## Model free learning with a model 

Learn a model and use the model to generate unlimited synthetic data. One of the biggest weakness is high variance but if you can generate the samples from your model, this problem is mitigated. 

---
*Page12*
Multiplying jacobians you inflate high eigenvalues and decrease small eigenvalues. 

---
*Page13*
[[Dyna]]
This is an online [[Q-Learning]] method. 

Original algorithm.

Step 5: 
Train more the Q-function by sampling from your model and perform an update on it. You can repeat it this step many times. 

---
*Page13*
We are gonna discuss class of methods that are used in practice. 
This does not have to be an onlie algorithm. 

3. Repeat K timesL
- sample a state from observed trajectory
- apply the same action (closer to in distirbution) or action from the latest policy (closer to on policy: tradeoff between distributional shift of model and policy ) , or random
- simulate next state
- train one (s,a,s',r), with actor critic or q learning algorithms
- you can take small roll outs 

Small rollout are advantegeous because distributional shift accumulates over time (branching from real roll-out).

---
*Page14*
models of really complex systems are good for short roll-outs. 

## Local Model

Another option is to use simpler policies. 

---
*Page19*
it a local model, is a model that is valid in the neighboorhood of the trajectory. if you don't know the model you need df/dx and df/du. 

---
*Page20*
At every step make a linear fit to your dynamics, a linearization at every timestep. 

---
*Page21*

Current controller from LQR ($p(u|x)$)

The fitting of the dynamics can be done by finding A, B matrix. 

This is fitted LQR with linear models, this controller gives you feedback controller. 

---
*Page22*
What do all the terms of the iLQR controller mean?

Version 0.5: open loop controller, small mistakes will not be corrected,

Version 1: iLQR law, better but if the system is (close) deterministic, if you collect 5 trajectories you gonna have 5 very similiar trajectories. Then you linear regression will be hill conditioned, you need to observe how the system react to different inputs. 

A better choice could be to inject some noise into the system (this is an idean very similiar to $\epsilon$-greedy algorithm). 

If many actions are good then you want a high variance, if there are a lot of bad actions you might want your policy close to deterministic (similiar to [[Boltzman Exploration]]).

---
*Page25*
Far away from the trajectories, the dynamics are non linear and you are out of distribution. The controller in these regions might not be able to correct for the error. 

So you need to contrain the controller in this setting? 
In [[Advanced Policy Gradients (9)]], a distributional shift in the state can be correct with contraints in the action distribution. 
So we can impose a contraint on the state distribution that require new trajectories to be close to old ones: KL divergence on the new trajectories and old ones. 

---
*Page28*
In reality we want ot use Deep RL to use a global policy can be used from any starting position of the robot. 

---
*Page29*
This idea to combine local polity to make global policy is very useful. 

---
*Page32*
[[Guided policy search]] 
The addition of the lagrange multiplier is equivalent to providing a KL contraint on the global policy wrt the local policies

---
*Page33*
[[Distillation]]

---
*Page34*
[[Devide and Conquer RL]]