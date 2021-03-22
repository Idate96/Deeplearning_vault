#cs287
#lecture

(Page 20)
When you have time try to prove theorem/facts. 

(Page 28)
Here we introduce: policy evaluation. We evaluate the current policy (without modifying it).

At convergence the value function will satisfy the Bellman Equation. 

(Page 31)
You can either run policy evaluation iteratively or solve the system of equation since they are linear in the transition probabilities and rewards. 

(Page 32)

## Maximum Entropy Formulation 
If the world changes, your found optimal choice might not work anymore. 
Is there any way to solve over a distribution of solutions (instead of a single one)?

(Page 36)
It measures the uncertainty of a random variable X.

(Page 37)
Now we want a policy that maximizes both cumulative discounted rewards and the entropy of the policy. This encourages the discovery of more than one strategy. 

[[Constrained Optimization]] can be used to solve the [[Maximum Entropy problem]].

(Page 54)
MPD were solved with [[linear programming]].

Minimize a positive weighting of the values of the states subjected to a constraint that is equivalent to the update in value iteration 
![[Screenshot 2020-10-30 at 08.28.09.png]]

(Page 58)
Every optimization problem has a [[dual optimization]].

