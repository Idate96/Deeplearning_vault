#cs287 
#lecture 
(Page 5)
Assume a system:
$x_{t + 1} = A_{x_t} + B u_{t}$
and a quadratic cost function:
$g(x_t, u_t) = x_t^{T} Q x + u_t^{T} R u_t$
where $Q > 0$ and $R > 0$ [[positive definite]] (if there is a negative value your optimization problem will go to minus infinity) and [[symmetric]] (only the symmetric part is used anyway). 

Q: *What happens if you pick a non-symmetric Q-matrix?*
$x^T F x$
By adding and subtracking the transpose we can write F as:
$F = \frac{F + F^T}{2} + \frac{F - F^{T}}{2}$
So we have 
$x^T + \frac{F + F^T}{2} x + x^T \frac{F - F^T}{2} x$

Remembering that $(ABC)^{T} = C^{T}B^{T}A^{T}$, and that the scalar value of a transpose is the scalar value itself,  the second term can be written as 
$\frac{x^T F x}{2} - (\frac{x^T F^T x}{2})^T = \frac{x^T F x}{2} - \frac{x^T F x}{2} = 0$
So if you are not using symmetric matrices in reality you are just using the symmetric part of your matrix (term 1).

(Page 7)
You can use this machinery to build automonous helicopters

(Page 8)
$J_{i + 1}(s) = min_u (g(s, u) + \sum_s' P(s'|s, u) J(s'))$
Cost to go at the next time step, is the current cost plus the expeced value the cost at the next state (under policy u) minimized over u. 

If we use the LQR objective:
$J_{i + 1}(x) = min_u x^T Q x + u^T R u + \sum_{x' = Ax + Bu} J(x')$
But since our dynamics are deterministic we can directly write it as:
$J_{i + 1}(x) = min_u x^T Q x + u^T R u + J_{i}(Ax + Bu)$

We initialize the cost to 
$J_0(x) = x^T P_0 x$
The first iteration:
$J_1(x) =  min_u x^T Q x + u^T R u +(Ax + Bu)^T P_0 (Ax + Bu)$
To find the minimum we take the gradient 
$\nabla_u J_1(x) = 2 R u + 2 B^T P_0 (Ax + Bu) = 0$
So the solution is 
$u = - (R + B^T P_0 B)^{-1} B^T P_0 A x = K x$
Now we can feed the u into the value:
$J_1(x) =  x^T P_1 x$
with 
$P_1 = Q + K_1^T R K_1 + (A + B K_1)^T P_0 (A + B K_1)$

**The amazing thing that is happening here is that $J_1(x)$ is quadratic in x, so we can repeat the same procedure just by changing the system**

In continous spaces this is the only case where an analytical solution is possible. 

(Page 12)
Objective keeping the liear system at the all-zero state. But there are many extensions!

(Page 13)
The objective now is to stay at state c. You can redifine the state space, to avoid accumulating cost being at state c. 

(Page 14)
Now we have to work with stochastic system. 

(Page 15)
Assuming there is a fixed point, can we stabilize around that point?
We can linearize our system around the fixed-point. 

(Page 16)
You might want to penalize for the time derivative of the control input. With high frequency controls, you are inputting high frequencies into the system, which often are not correctly modelled. Therefore your controller is giving very high gain, high frequency inputs that might destabilize your system (or brake it apart).

Solution: is to frequency shape the cost function or put a filter

(Page 19)
First row are the original dynamics, and second row integrator of the control input. 
**In practice it almost never work if you don't do something similiar with your system**

(Page 20)
For time-varying linear systems. you can approximate a non-linaer system in this way. 

(Page 21)
For example for trajectory following for an elicopter. Assume you know a feasable sequence of states and the associated control inputs. Usually there perturbation to your system, so you want to minimize the quadratic devition from your trajectory and precomputed (open-loop) inputs. The sequence of inputs serves as strong prior to the actual inputs used. 

Q: *What if you don't know the u necessary for the trajectory?*
You can set $u* = 0$. The control inputs could be obtained from a demostration. 

Q: *Does the error accumulates?*
If the perturbation is very strong, you might become unstable. 

(Page 23)
Q: *What if you try to use a convex solver for this problem?*
You will obtain the control input (u) but not the feedback controller. 

(Page 25)
For the general case you can linearize the dynamize and a second order approximation of the cost function. 

(Page 27)
Make sure your linearization are valid (trust regions). Make sure you stay close to the previous trajectory, where you can trust your gradients. You can do this by adding a factor in the cost function. 

(Page 28)
If Q and R are not positive definite you can increase the $\alpha$ until they are positive definite 

(Page 30)
Don't understand this term 

(Page 31)
For the elicopter problem, we selected around 40 step look ahead, because if 40 steps you could predict to be back close to the trajectory. Once you are close to the trajectory you can trust your value function. 

(Page 32)
In reality this is quite common, for example even in humans (multiplicative noise a source of crossover regression?)

(Page 36)
Trick to bound the input, is to use $u = tanh(v)$, but is still beneficial to keep v close to zero. Gradients far out are almost zero and it will be hard to bring back the value of v.  

(Page 37)
This is theory to support things we already know work. If the the equilibrium point is stable for the linearized system then is stable for the non-linaer system (remember first year course on non-linear system). [[Lyapunov theory]]

(Page 38)
[[Controllability]] primer. 
If your LQR system is not controllable, you will not manage to stabilize your system. 

(Page 39)
[[Feedback linearization]] is all about finding a global transformation to linearize your non linear system. 

(Page 41)
Example of feedback linearization when hte number of control inputs does not match the dimension of the state space. 

(Page 48)
Maybe you can learn the transformation
