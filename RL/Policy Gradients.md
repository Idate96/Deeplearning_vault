#cs287 
#lecture 

*page 10*
By having stocastic policies we get a smooth optimization problem, even in the case of discrete actions. We can gradually shift the probability from one action to another 

---
*page 13*
Auxiliary objective can help you to learn fast, because the RL reward is spars.  

---
*page 14*
Methods like iLqr, MPC are policy optimization methods where the dynamics are available. 

---
*page 16*
[[Finite difference]] estimate of gradient over $\theta$. In this method you just need rollouts. 

---
*Challenges*:
system is stocastic: even though the expected return is higher you might sample low values. This misinforms the gradients. This method is very susceptible to high variance. 

---
*page 17*
To reduce the variance we can average over multiple rollouts, but you pay a higher cost. 

---
*page 18*
If you have access to the simulator, you can fix the random seed!

---
*page 22*
12 numbers of parameters for the policy -> possible to train with finite differences to get a good controller. 

---
*page 31*
A form of an evolutionary methos is the [[Cross Entropy Method]]. 


## Policy gradients 
*Notation*
The trajectory $\tau = (s_0, u_0, ..., s_H, u_H)$
Objective $U(\thata) = E[\sum_{t = 0}^H R(s_t, u_t); \pi_{\theta}] = \sum_{\tau}P(\tau; \theta)R(\tau)$

---
*page 35*
Before we had a single trajectory, and we tried to make it better by shifting it. zz
Likelihood ratio policy gradients: 

$$
\begin{align}
\nabla_{\theta} U(\theta) = \sum_{\tau} \nabla_{\theta} P(\tau; \theta)R(\tau) \\
= \sum_{\tau} \frac{P(\tau; \theta)}{P(\tau; \theta)} \nabla_{\theta}P(\tau; \theta)R(\tau)\\
= \sum_{tau} \P(\tau; \theta) \nabla_{\theta} log(P(\tau; \theta)) R(\tau) \\
\approx \frac{1}{m}\sum_{i = 0}^m \nabla_{\theta}P(\tau^(i); \theta)R(\tau^(i)) \\
\end{align}
$$

Now we would like to obtain an expression for the gradient of the probability of trajectory:
$$
\begin{align}
\nabla_{\theta} log(P(\tau^(i); \theta)) = \\
\nabla_{\theta} log [P(s_0) \prod_{i = 0}^H P(s_{t + 1}|s_t, u_t) \pi(u_t|s_t)] \\
= \nabla_{\theta} \sum_{t = 0}^H log(P(s_{t + 1}|s_t, u_t)) + \nabla_{\theta} \sum_{t = 0}^H log(\pi(u_t|s_t)) \\
= 0 + \sum_{t = 0}^H \nabla_{\theta} log(\pi(u_t|s_t)) 
\end{align}
$$
This last quantity is available with pytorch, tensorflow ... 

## Importance sampling 
It can give us a local approximation of the objective upon which we do multiple steps on. 
The basic idea is to change the policy $\pi(\theta)$ while we still take multiple samples from $\pi(\theta_{old})$

$$
\begin{align}
U(\tau) = E_{tau \sim \theta}[P(\tau|\theta)R(\tau)] \\
= E_{tau \sim \theta_{old}}[\frac{P(\tau|\theta)}{P(\tau|\theta_{old})}R(\tau)] \\
\end{align}
$$

This implies 
$$
\begin{align}
\nabla_{\theta} U(\theta) = E_{tau \sim \theta_{old}}[\frac{\nabla_{|theta} P(\tau|\theta)}{P(\tau|\theta_{old})}R(\tau)] \\
\end{align}
$$
If we evaluate the gradient at $\theta = \theta_{old}$ we recover the same formualtion as before. 

$\nabla_{\theta} U(\theta)|_{\theta{old}}e = E_{tau \sim \theta_{old}}[\nabla_{\theta} logP(\tau|\theta)|_{\theta_{old}} R(\tau)]$

In practice this formulation is naive and the sample estimate is high variance

### Baseline Subtraction 
*page 57*
I want to increase the probabilities of actions given state only if it's better than the baseline. Subtracking the baseline from the reward does not change the gradient! 

### Temporal Structure 
*page 58*
For each roll out we sum for each time step the current policy gradient times the reward obtain from **that moment in time till the end**. 

$\nabla_{\theta} U(\theta) = \frac{1}{m} \sum_{i = 0}^m \sum_{t = 0}^{H-1} \left( \nabla_{\theta} \log(\pi_{\theta}(a_t|s_t)) \right) \left(\sum_{k = t}^{H-1}R(s_k, u_k) - b(s_t) \right)$

---
*page 59*
Different choices for the baseline:
- Constant 
- Choise that minimize the variance of the objective: weighted average of rewards
- Time dependent: look at things only forward in time 
- State dependent: [[value function]]. This is the most common choice

---
*page 61*
We could use [[Value Iteration]] but it requires a model of the environment. 
Otherwise we can regress using [[Monte Carlo]], by training a neural network, against the empirical estimate of the value function, using least square methods. 
This is a supervised learning problem. 

---
*page 62*
We can also do bootstrapping! But it's not super stable, at the beginning to get something to run use monte carlo. 


## Advantage Estimation 
The idea is to improve the estiamtion of the advantage function by making use of [[TD(N)]] and [[Eligibility Traces]].

We estimate the advantage by making use of value functions 
$A(s_t, u_t) = \sum_{k = t}^{H-1} (R(s_k, u_k) - V^{\pi}(s_k))$
Instead of using montecarlo we can use TD-Learning to approximate the reward 
$Q^{\pi}(s, u) = E[\sum_{t = k}^{H-1}r_k|s_k, u_k]$
therefore with TD(1)
$Q^{\pi, \gamma}(s, u) = E[r_0 + \gamma V^{\pi}(s_1)|s_0 = s, u_0 = u]$

The [[A3C]] or [[GAE]] algorithms is 
Initialize $\pi_{\theta}$ and $V^{\pi}_{\phi}$
Collect rollouts $(s, u, s', r)$ and compute for each roll out 
$Q_i(s, u) = r + \gamma V^{\pi}(s')$

Then update the value function and the policy:
$\phi_{i + 1} = \min_{\phi} \sum_{s, u, s', r}|| r + \gamma V^{\pi}_{\phi_i} - V^{\pi}_{\phi_i}||^2_2 + \lambda||\phi - phi_{i}||$
$\theta_{i + 1} = \theta_i + \alpha \frac{1}{m} \sum_{i = 0}^m \sum_{t = 0}^{H - 1} \nabla_{\theta} \log(\pi(u_t|s_t)) (\sum_{k = t}^{H-1}Q_i(s_k, u_k) - V^{\pi}(s_k)$)

## Trust Region 
[[Trust region policy optimization]] explanation.

---
*page 85*
Step sizes are really important, because if you step too far you land on a terrible policy and now the data is collected on this terrible policy. No automatic self-correction happens in RL

---
*page 86*
Q: *Why not use line search?*

---
*page 87*
The [[KL-Divergence]] can define a trust region

---
*page 88*


$$
\begin{align}
KL(P(\tau; \theta)|P(\tau; \theta + \delta \theta)) = \sum_{\tau} P(\tau; \theta) \log \frac{P(\tau; \theta)}{P(\tau; \theta + \delta \theta)} \\

= \sum_{\tau} P(\tau; \theta) \log \frac{P(s_0)\prod_{t} P(s_{t + 1}|s_t, u_t)\pi_{\theta}(u_t|s_t)}{P(s_0)\prod_{t} P(s_{t + 1}|s_t, u_t)\pi_{\theta + \delta \theta}(u_t|s_t)} \\

= \sum_{\tau} P(\tau; \theta) \log \frac{\prod_{t}\pi_{\theta}(u_t|s_t)}{\prod_{t} \pi_{\theta + \delta \theta}(u_t|s_t)} \\

= \sum_{\tau} P(\tau; \theta) \sum_t \log(\pi_{\theta}(u_t|s_t)) - \log(\pi_{\theta + \delta \theta}(u_t|s_t)) \\

\approx \frac{1}{M} \sum_m \sum_t \log(\pi_{\theta}(u_t|s_t)) - \log(\pi_{\theta + \delta \theta}(u_t|s_t)) \\

= \frac{1}{M} \sum_{s, u} \log \frac{\pi_{\theta}(s, u)}{\pi_{\theta + \delta \theta}(s, u)}

\end{align}
$$
We have access to this values in our rollouts

---
*Page 93*
One way to enforce this for neural network is to approximate the KL divergence with a second order approximation.
To compute the [[Hessian]] (which happens to be the [[Fisher Information Matrix]]) we only need gradients. 

Now we have a linear problem with a quadratic contraint!

---
*page 104*
This algorithm allowed to have a stable algorithm that can reliably optimize the objective. 


---
*page 111*
[[PPO]] is the best method if the data collection is very fast. 


---
*page 112*
Q:*Why not happy with [[Trust region policy optimization]]?*

[[Conjugate Gradient]] is complex, so it would be better to use standard optimizers. 

---
*page 113*

TRPO 
$$
\max_{\theta} E_t[\frac{\pi_{\theta}(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)} A_t]
$$
subject to $E[KL[\pi_{\theta_{old}}(.|s_t)|pi_{\theta}(.|s_t)]] < \epsilon$

Let's try to turn it into uncontrained optimization problem with [[Lagrangian approach]]:
$$
\max_{\theta} E_t \left[\frac{\pi_{\theta}(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)} A_t \right] - \beta (E[KL[\pi_{\theta_{old}}(.|s_t)|pi_{\theta}(.|s_t)]] - \epsilon)
$$

Q:*How do we find the right $\beta$?*
We use [[Dual Descent]]

$$
\min_{\beta} \max_{\theta} E_t \left[\frac{\pi_{\theta}(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)} A_t \right] - \beta (E[KL[\pi_{\theta_{old}}(.|s_t)|pi_{\theta}(.|s_t)]] - \epsilon)
$$

Our $\beta$ will go up or down depending if we satisfy the contraint or not (if satisfied small otherwise high). The update alternates between $\theta$ and $\beta$.
You can use [[Adam]] or [[RMSprop]] for this kind of problem. 

---
*page 115*

Define $r_t(\theta) = \frac{\pi_{\theta}(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)}$ so $r(\theta_{old}) = 1$

The new objective is 
$\max_{\theta} L^{CLIP}(\theta) = E_t[\min(r_t(\theta)A_t, clip(r_t(\theta), 1 - \epsilon, 1 + \epsilon)A_t)]$

The minimum is making sure that you are never more optimistic that the original objective, it's a conservative estimate. 