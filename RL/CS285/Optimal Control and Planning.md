#cs285 
#lecture 

First let's learn how a model is used for control and planning. In today's lecture there will not be any learning at all. 

*Page 5*
2. Some systems are easily to manually model, like the kinematics of a car (in good weather)

If we know the dynamics, we can use very powerful algorithms

---
*Page 6*
Today we consider the case of choosing action under perfect knowledge 

---
*Page7*
objective:
$\min_{a_1, ..., a_T} \sum_{t = 1}^T c(s_t, a_t)$ subject to $s_t = f(s_{t-1}, a_{t - 1})$, the constrained encode the dependency of future states to the past state and action.
This can also be expanded into the stochastic case using expectations. 

---
*Page9*
Given a set of chosen actions:
$p(s_1, ..., s_T|a_1, ..., a_T) = p(s_1) \prod_{t = 1}^Tp(s_{t + 1}|s_{t}, a_{t})$
We are conditining everything on a plan (a sequence of action).

$a_1, ..., a_T = arg, \max_{a_1, ..., a_T} E[\sum_t r(s_t, a_t)|a_1, ..., a_T]$
In come cases in a stochastic setting this objective can be extremely subobptimal. Since the plan is thoght beforehand you cannot actually change your actions when you actually observe the next state, which might be different from your expected state. 

*Example*: "Self-driving car that has to turn right, but suddenly there is another car trying to steer that was not seen before"\

The whole problem is open loop behaviour in a stocastic environment. 

*Example*: solve an exam before having the questions revealed
The optimal action might be actually to not take the exam

---
*Page11*
There are many choices for the policy. A NN is a global policy that tells the agent what to for every state. You can near optimal closed loop planning with time-varying linear feedback policies, if you stay close to the intended trajectory.

**Local vs Global**

---
*Page14*
### Black box optimization method 
we abstract away the temporal aspect of the problem 
$a_1, ..., a_T = A = arg, \max_{a_1, ..., a_T}J(a_1, ..., a_T) = arg \max_{A} J(A)$
The algorithm does not ever care that the actions are a sequence (order).

*Random shooting methods* seem very ineficient but for low dimensional states they could be useful. 
One advantage of this approach is that is super simple to implement and efficient on the hardaware.

---
*Page16*
[[Cross Entropy Method]], CMA-ES is a more advanced version. 
You are relying on the sampling to cover the space of plans, so if you have more than 30/60 dimension it's gonna be a problem.

For example: 10 dimensional problem with 15 timesteps -> 150 dimensional problem 

---
*Page17*
[[Monte Carlo Tree Search]] are very difficult to analyze theoretically but they work very well in practice

---
*Page23*
We can write our [[Constrained Optimization]] without constraints by substituting for the state $x_t$:

$min_{u_1, ... u_T} c(x_1, u_1) + c(f(x_1, u_1)), u_2) + ...$

We can take the derivative of both the cost and the dynamics with respect to the action and the state. 

In practice using a second order method really helps (which method? [[Newton's Method]]).

---
*Page 23*
[[Shooting methods]] vs [[Collocation methods]]

---
[[LQR (CS287)]]
The linear case means that the dynamics are linear:
$f(x_t, u_t) = F_t \begin{bmatrix} x_t \\u_t \end{bmatrix}+ f_t$
where $f_t$ is a constant vector.

The cost is quadratic (at least), but it can have a linear term. 

We can have a different dynamics and cost at each time step 

---
*Page27*
First we solve for the last action $u_T$. 
We terefore consider only the cost at the last action $Q(x_T, u_T)$
We also break up the matrix $C_T$ into subcomponent that depend on the action and input.

We can find the optimal action by taking the derivative of the cost wrt to the action and setting it to 0. 

---
*Page28*
Now that we have an expression of the action as function of the state $u_T \approx K_T x_T$ we can find the [[value function]] by substituting the value of u in the [[Q-Function]].

---
*Page30*
Solve for the previous time step. 
Just like before we can take the gradient and we obtain an identical form for the solution. 

---
*Page31*
The algorithm first compute the final actions and going backwards. 

---
*Page34*
if the stochastic dynamics are gaussian (with constant covariance), the previously derived control actions are optimal. 

Adding gaussian noise, change the state that you visit. Therefore you cannot produce a single open loop sequence of action, but you can still formulate an optimal policy (linear) wrt to the state. This policy is the optimal closed loop policy.

---
*Page35*
With a taylor expansion we are back in teh linear regime and solve for the best $\delta x$ and $\delta u$.


---
*Page38*
For the forward pass we use the real non-linear dynamics. 

---
*Page 39*
iLQR differes from [[Newton's Method]] in the fact that is not considering second order dynamics (like DDP does).

---
*Page40*
We want to check if our new solutions (red line is approximated function) (red cross) is actually better than the old solution and if not move closer to it. This is related to the idea of [[Trust region method]].

We can apply this principle at line 5 when we pass the forward pass and compute $u_t$, changing the costant term $k_t$ to $\alpha k_t$. 
This constant if set to 0, at step 1 we would have that $u_t = K_t (x_t - {x_t}') + \alpha k_t + {u_t}' = {u_t}'$. Then if the action is the same as before also the state is not gonna change w.rt. the previous iteration of the iLQR -> the policy will not change throught the run. 
You can then search for the $\alpha$ for example with [[Braketing line search]]

**To clarify is it possible to do iLQR online**

---
*page42*
This is basically MPC. 

---
 

