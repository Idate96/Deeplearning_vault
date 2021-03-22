#chapter 

Like DP, TD methods don't wait until the end of the episode to update estimates (unlike [[Monte Carlo]] methods.)

## TD(0)
The idea behind 0 is that you wait just until the next step to update the estimates. The update rule is

$V(S_t) = V(S_t) + \alpha (R_{t + 1} + \gamma V(S_{t + 1}) - V(S_t)))$

and we define the temporal difference error to be

$\delta_t = R_{t + 1} + \gamma V(S_{t + 1}) - V(S_t)$

which is the error in the estimate at that time

![[TD_0_algo.png]]

Its [[backup diagram]] is 
![[TD_0_backup.png]]


## Sarsa
#onpolicy 
It uses $(S_t, A_t, R_{t + 1}, S_{t + 1}, A_{t+1}))$ to do a TD update on the [[action-value]] estimates:

$Q(S_t, A_t) = Q(S_t, A_t) + \alpha (R_{t + 1} + \gamma Q(S_{t + 1}, A_{t + 1}) - Q(S_t, A_t)))$

At a given time step you select the future actions as well:

![[sarsa.png]]

## Q-Learning 
#offpolicy 
The Q estimate directly approximates the optimal estimate $q_{*}$, indipendently of the policy that is currently used. 

$Q(S_t, A_t) = Q(S_t, A_t) + \alpha (R_{t + 1} + \gamma max_a Q(S_{t + 1}, a) - A(S_t, A_t)))$

Infact the $a_{*} = arg\,max_a Q(S_{t + 1}, a)$ is not necessarily the selected action $A_t$ (usually $\epsilon$-greedy policy). 
![[Screenshot 2020-10-22 at 10.35.15.png]]

Unlike Sarsa, Q learning does not take into account the effect of action selection (say epsilon greedy policy), this is shown in the learned policy for grid world:
![[Screenshot 2020-10-22 at 10.36.57.png]]

## Expected Sarsa 
#onpolicy 

$Q(S_t, A_t) = Q(S_t, A_t) + \alpha (R_{t + 1} + \gamma E_{\pi}[Q(S_{t + 1}, A_{t + 1})|S_{t + 1}] - Q(S_t, A_t)))$

![[Screenshot 2020-10-22 at 10.35.24.png]]

