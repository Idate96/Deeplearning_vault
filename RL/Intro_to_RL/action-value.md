#definition
An action-value function is the expected reward for having taken that action:
$q(a) = E[R_t|A_t = a]$

## Estimation 
Averaging rewards:
$Q_t(a) = \frac{\sum_{i = 0}^{N}R_i \cdot 1_{A_i = a}}{\sum_{i = 0}^N 1_{A_i = a}}$

and with incremental implementation (assuming only action A is available)

$Q_t(A) = \frac{1}{n} \sum_{i = 0}^N R_i = Q_{t - 1}(A) + \frac{1}{N(A)}(R_i - Q_{t - 1}(A))$

if the environment is non-stationary we want to give more weight to more recent rewards by choosing a constant stepsize for the update (exponetially weighted average reward):
$Q_t(A) = Q_{t-1}(A) + \alpha (R_t - Q_{t - 1}(A))$

