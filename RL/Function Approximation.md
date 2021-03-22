#chapter 
Function approximations are necessary since tabular methods will become incractabily large (think about images).

The [[value function]] can be parametrized as $v(s, w)$. 

## Linear function approximation 

$v(s, w) = \sum_i w_i x_i(s)$

There are several ways to define features $x(s)$, like a one-hot encoding (one state per each feature).

## Prediction Objective
Now that we are using function approximation we need to specify an objective, given a set of states and value function we want to tune the weight. 
![[Screenshot 2020-10-22 at 11.16.24.png]]

The square error is a good [[loss function]], adding how much we care about specific subset of values 

$VE = \sum_s \mu(s) (v_{\pi}(s) - v(s, w))^2$

The weighting can be chosen proportional to the time spent in that state under policy $\pi$.

## Optimizer 
To mimize our loss function we can use an optimizer as [[SGD]]. It's update rule is 

$w_{t + 1} = w_t + \alpha (v_{\pi}(S_t) - v(S_t, w_t))) \nabla_w v(S_t, w_t))$

This update rule relies on the assumption that the *target value is indipendent of the weight **w***. 

![[Screenshot 2020-10-22 at 11.25.49.png]]

This assumption is often not valid, in this case we call the algorithm *semi-gradient descent*. For example in TD(0) semigradient the target $v_{\pi}(S_t) = R_{t} + \gamma v(S_{t +1}, w)$

![[Screenshot 2020-10-22 at 11.30.00.png]]

## Features with Tile-Coding 

![[Screenshot 2020-10-22 at 11.34.53.png]]