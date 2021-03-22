Q-Learning in continous spaces, we cannot output a Q-value for every action.
It's also gonna introduce entropy bonuses. 

We have a parametrized policy $\pi_{\phi}(a_t|s_t) \propto exp(Q_{\theta}(s_t, a_t))$, in this way we turn q values into probabilities.

Objectives

$$
\min_{\phi} KL(\pi_{\phi}(.|s_t)|| exp(Q_{\theta}(s_t, .)))
$$
$$
\min_{\theta} \sum_t \left( Q_{\theta}(s_t, a_t) - r_t + E_{s_{t + 1}}[V_{\psi}(s_{t + 1})] \right)^2
$$
$$
\min_{\psi} \sum_t \left( V_{\psi}(s_t) - log( \int exp(Q_{\theta}(s_t, a_t)) d a_t) \right)^2
$$

The integral is estimated by sampling the actions and empirically evaluating the integral. 

This algorithm is very robust across the space (unlike normal Q-learning) and introduces noise during training. 