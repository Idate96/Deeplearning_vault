# Markov Decision Processes
#definition 
#chapter
 
 MDP are formalizatoin of a decision making, where action does not influence just the immediate reward but also future state, actions and reward. 
 
 MDP are constitute by a set of state, actions and rewards (S, A, R)
 ![[MDP_scheme.png]]
 
 The environment can be characterized by [[environment dynamics]].
 
 The agent maximezes the [[expected return]] in case of finite or infinite episodes. The [[reward hypothesis]] formalizes this idea.
 
 
## Policy and value function
The action preference of the agent, which are encoded in a [[policy]]. Given a policy $\pi$ the measure of how good is a state is the [[value function]].

The objective is to obtain an optimal policy and [[value function]] define an ordering over policies: 
$\pi' > \pi$ iff $v_{\pi'}(s) > v_{\pi}(s)$ for all $s \in S$.

The optimal value function and optimal action value function are given by

$v_{*}(s) = \max_{\pi} (v_{\pi}(s))$

$q_{*}(a, s) = \max_{\pi} (q_{\pi}(a, s))$

We can aslo find the [[Bellman equation]] related to them:

$v_{*}(s) = \max_{a \in A(s)} q_{\pi*}(a, s) = \max_a \sum_{r, s'} p(s', r| a, s)(r + \gamma v_{*}(s'))$
![[backup_bellman_value.png]]
and 
$q_{*}(s, a) = \sum_{r, s'} p(s', r| a, s)(r + \gamma \max_{a'} q_{*}(s', a'))$

![[backup_bellman_action.png]]