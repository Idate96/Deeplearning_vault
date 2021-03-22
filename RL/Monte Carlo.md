#chapter
*Montecarlo methods, contrary to [[Dynamic Programming]] that needs a model of the world, relies only on experience*

## Monte Carlo prediction
Objective is to estimate the [[value function]]. 
![[MC_prediction.png]]

Note: if the reward is given only at the end, first the agent must be able to reach it and then in each episod backpropate the value function state by state. 

Its [[backup diagram]] is given by 
![[MC_backup.png]]

Advantages over DP:
- All probabilities must be computed before applying DP, somethings even in complete knowledge of the environment MC is attractive
- MC estimates for each state are indipendent (clarify)
- computational expense of MC is indipendent of the number of states


## Monte Carlo for action-value functions
Without a model we are forced to estimate [[Q-Function]]. If $\pi$ is a deterministic policy then the agent will never explore the environment. A solution to this problem is *exploring starts* where the agent is forces to start in different states to garanthee full expolratio of the space x action space. 

Notice how the update of the estimates is done backwards in time!

![[MC_es.png]]

## Monte Carlo without exploring starts 
#onpolicy
Another [[On policy method]] uses $\epsilon$-soft policies: 
$\pi(a|s) > 0$ for all $a \in A(s)$

You can observe in the algorithm below how the probability is divided between the optimal greedy action and the other actions.

![[onpolicy_MC.png]]

## Importance Sampling 
#offpolicy
This is an [[Off-policy method]]

Target policy: $\pi(a|s)$
Behaviour policy: $b(a|s)$

The goal is to estimate $E_{\pi}(x)$ while sampling $x ~ b$:

$E_{\pi}(x) = \sum_{x} x \pi(x) = \sum_x x \frac{pi(x)}{b(x)} b(x) = \sum_x x \rho(x) b(x) = E_{b}(x \rho(x))$

The basic idea is to reweight the probability of a certain trajectory by multiplying it with a factorial of importance sampling ratios. 

![[off_policy_MC.png]]