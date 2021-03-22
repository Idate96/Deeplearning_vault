We know the [[Policy Gradient]] learning rule and if we use [[Temporal Difference]](0) to approximate the value function:

$\theta_{t + 1} = \theta_{t} + \alpha \ln(\nabla \pi(A_t|S_t, \theta_t))  (R_{t + 1} - \bar{R} + v(S_{t+1}, w))$

The critic is the value function while the actor the policy. 

We can also add a baseline to the gradient to reduce its variance and increase the speed of learning:
![[Screenshot 2020-10-22 at 16.27.28.png]]

A diagram of the algorithm:
![[Screenshot 2020-10-22 at 16.28.35.png]]