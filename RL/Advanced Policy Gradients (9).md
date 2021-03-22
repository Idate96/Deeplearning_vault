#cs285 
#lecture 

(Page 2)
Reward to go can be computed with [[Monte Carlo]] or learning a q-function. 

(Page 3)
Policy gradients moves slightly the policy towards giving more probabilities weighted by the advantage estimator (instead of just taking the arg max).

(Page 4)
$J(\theta') - J(\theta)$ represents the improvemnt in the RL objective my updating the policy. 

Claim: the difference represent the imporvement in the RL objective going to a new policy. So if you make this difference large you can make a large improvement. 

Idea of the proof: maximizing the right end side (cumulative advantage rewards) implies that you maximize $J(\theta') - J(\theta)$ which implies that you maximize $J(\theta')$ as it's the only term that depends on the current parameters.

**Objective**: is to show that maximizing the advantage of the old policy, under the new policy (in expectation) you are maximizing the new policy.  

Line 2: change the distribution to one that has the same marginal over the inital state($p(s_0$)) (Not clear). I  guess the trajectory all start from the same state so it does not matter. 
	
(page 5)
If we actually wanna evaluate the expectation we need to use importance sampling. 

(page 6)


(Page 8)
We want to ignore the distribution mismatch, so that $\theta'$ appears only in the importance sampling weight. In this way taking the gradient of the right hand side recovers policy gradients. 

(Page 9)
State marginal are close if the policies are close. 

First term probability that old policy does the same as new policy. 

(Page 10)
if you have two distribution and the total variation divergence is $\epsilon$ then you can contruct a joint distribution of two variables ...

(Page 12)
We proved that maximizing the rhs, maximizes the right RL objetive as long as the updated policy is close to the previous policy 

![[advanced_policy_gra.png]]

## Implementation 
(Page 14)
Total variation divergence is related to the KL divergence.
Total variation diverenge is not differentiable, therefore KL is much easier to use. 
Contraints therefore will be expressed using the KL divergence, which is also differentiable everywhere 

(Page 15)
How do you enforce the closeness contraint on the policy?

(Page 16)
[[Dual Descent]] is a techique to find the right lagrange multiplier to solve your constrained optimization problem. 

## [[Natural Gradient]]
Another way to impose the contraint that leads to a simple algorithm. 

**Basic Idea: linearize your objective and minimize it while staying inside your trust region (contraint)**

When taking gradient ascent, it's equivalent to optimizing the linearization of the function. 

We need to simplify the expression for the gradient.

(Page 19)
if you evaluate at $\theta' = \theta$ the importance weight cancel out and we obtain back the regular policy gradient [[Reinforce]].

(Page 20)
Q: *Do we updated the parameters by taking a gradient step?*

Taking a step of gradient ascent will lead often to the violation of KL divergence contraint since there parameters that change the policy much more, respect to others. 

The step for gradient ascent can be obtained as a lagrange multiplier, for the contraint and you can find a closed form equation. To check that it is true just substitute back 

$\sqrt{\frac{\epsilon}{||\nabla_{\theta} J(\theta)||^2}} \nabla_{\theta}J(\theta)$ into the contraint $||\theta - \theta'||^2 < \epsilon$

A constrain in $\theta$ space will not mean that the contrain is satisfied in KL space. 

Grandient ascent forces a contraint in the parameter space but not in the probability space. 

(Page 21)
To solve our problem we have so expand the KL divergence. 

We don't use 1 st order approximation because the first derivative of KL is 0 so we use a second order expansion. 
You can approximate the fisher information matrix with samples. 

(Page 23)
Q:*Is it a problem in practice?*


(Page 24)
you can use the conjugate gradient to compute the product of fisher information matrix and the gradient, more details in [[Trust region policy optimization]].




