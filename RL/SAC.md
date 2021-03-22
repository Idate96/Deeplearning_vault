A more stable version of [[DDPG]].

The objective is 
$$
\begin{align}
J(\pi_{\phi}) = \sum_{t = 0}^T E_{(s_t, a_t) \sim \rho_{\pi}}[r(s_t, a_t) + \alpha \mathcal(H)(\pi(.|s_t))]
\end{align}
$$

Iterate 
Rollouts from $\pi_{\phi}$ to the replay buffer


We are gonna learn a policy, a value function and a q function. 

![[Screenshot 2020-12-02 at 09.41.15.png]]

Compared to [[Soft-Q]], SAC tries to learn a better online Q function while the previous tries to learn the optimal. 

SAC is a very stable algorithms!