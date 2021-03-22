#cs287 
#lecture 

### Marginalization
Page 13
Given a going distribution, $p(x, y)$, how do we find $p(x)$?

$p(x) = \int p(x, y; \theta) dy$

Since the probability distribution is a gaussian we can actually carry out the integral, you can check the derivation on the slides using the completion of square trick. 

### Conditioning 
*Page 13*

We have $p(x, y)$, we observe $y = y_0$ we would like to have $p(x|y = y_0)$. Substitute the value of $y_0$ to obtain the distribtuion. 


## Kalman Filter 
It can be considered a specific case of a Bayes filter, but when the dynamics are linear and the measurment are also linaer. If your initial state is gaussian, $x \sim \mathcal(N)(\mu_0, \Sigma_0), given the condition above also the future variables will be gaussians. 

We have access to $z$, the measurment, but not $x$, the state. 
The graph is saying that the variable $x_{t + 1}$ just depends on $x_{t}$ and $u_{t}$

### Time update 
---
*Page 19*
Assume we have a belief about $x_{t}$,  $p(x_t|z_{0:t}, u_{0:t})$. We can therefore compute what is our expected $x_{t+1}$ based on a model of the dynamics of the system. 

$p(x_{t + 1}|z_{0:t}, u_{0:t}) = \int_{x_t} p(x_{t + 1}, x_t|z_{0:t, u_{0:t}}) dx_t$

We can rewrite it as:
$p(x_{t + 1}, x_t|z_{0:t}, u_{0:t}) = p(x_{t + 1}|x_t, z_{0:t}, u_{0:t})p(x_t|z_{0:t}, u_{0:t}) = p(x_{t +1}|x_t, u_t)p(x_t|z_{0:t}, u_{0:t}$
last equality comes from the graph (markovian property)
We assume that **we already know $p(x_t|z_{0:t}, u_{0:t})$** from the previous time-step. 

---
*Page 20*
Now we expand $p(x_{t + 1}|x_t, u_t)$

We know that 
$x_{t + 1} = A_{t}x_t + B_t u_t + \epsilon_t$
with $\epsilon_t \sim \mathcal{N}(0, Q_t)$

so it expands as shown in the slide (as a gaussian).

---
*Page 21*
Procedure to obtain an explicit joint distribution. 

The expected mean $\mu_{t|0:t}$ can be found easily. 

---
*Page 22*
By marginalizing the joint distribution we can obtain a distribution for the next state. 

### Observation Update
*Page 25*
We assume we have the probability distribution over the next state. 
The measurement is linearly related to the state plus noise. 

We then can contruct the joint probability distribution over the the observation and htbe next state using the formula of above.

Now we can condition $x_{t + 1}$ on the observation using the formulas for gaussians. 

## EKF 
Solution to handle non linear systems. It linearizes the dynamics at each time step.

---
*Page 34*
The high variance case is very poor, nontheless this case is very rare in practice. The linearization is much better in low variance cases.

## UKF
Another way to deal with non-linear systems. 

With ukf you keep all the terms of the non linear function (evaluating it at the sigma points) and approximate instead the target distribution.

---
*Page 45*
How to pick the points?
