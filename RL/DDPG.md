Precursor to [[SAC]] (hard version).

Rollouts: under the current policy (+ some noise for exploration)

We learn the Q-function of the current policy:
$$
\sum_t (Q_{\phi}(s_t, u_t) - \hat{Q}(s_t, u_t))^2
$$
with $\hat{Q}(s_t, u_t) = r_t + gamma Q_{\phi}(s_{t +1}, u_{t + 1})$

Then we do a policy update, that uses the assumption of *continous actions* (gradient of Q wrt the action)

$\theta = \theta + \alpha \sum_t \nabla_{\theta} Q_{\phi}(s_t, \pi_{\theta}(s_t, v_t))$

$v_t$ is noise

The off-policy version uses:
$$
\sum_t (Q_{\phi}(s_t, u_t) - \hat{Q}(s_t, \pi(s_{t + 1}, \pi(s_{t + 1}))))^2
$$

![[Screenshot 2020-12-02 at 09.31.53.png]]


