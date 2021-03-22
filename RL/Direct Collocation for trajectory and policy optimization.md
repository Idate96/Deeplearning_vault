#cs285 
#lecture 

## Trajectory optimization 

Page 14

You have a very narrow feasable region for shooting methods. You might have to take very roundabout ways to find the optimal solution. This would not happen if the dynamics were a soft constraint (Page 21), the optimizer could find a much shorter path. This does come up in practice quite often (Page 19).


---
Page 27
You can optimize directly for a trajectory of states as opposed to states and control. The controls can be recovered implictly using the inverse dynamics. 

---
Page 28
In this way the dependency is limited only to $(x_t, u)$. We avoid in this way forward integration that could be problematic due to numerical erros. If you use a higher order integretor, it is still a problem. 

---
Page 30

Imposing the dynamics as contraint gives us more freedom. For example we can set the dynamics as soft contraint. 


## Inverse dynamics model 
Page 35
We can learn this function from data but for rigid multi-body dynamics we can do better if we know the system parameters. 

### Rigid Multibody dynamics
$x^{t} = q^{t}$
and we can compute the velocities and acceleration can be computed with finite differences. 
We can bring all the terms of the dynamics equation to the right and minimize to find the control action to achive the specified next state (otherwise if there is an analytical form you can solve directly for the control)

---
Page 49 
Example for particle. 


## Numerical Optimization for collocation 
Can we do something similiar as to the shooting method and use a second order method to solve this problem?

Q:*Why second order?*
You are trying to solve a system of simultaneous contraints. if you are using first order methods, you can try to change your variable to satisfy one contraint and maybe unsatisfy another one. Second order methods or methods that consider simultaneously changing variables. 

![[Screenshot 2020-11-19 at 18.58.46.png]]


## Dynamics with Contact
---
Page 63
Contact introduces discountinous jumps in your energy landscape.
The dimensionality of f changes between two different states. 
Also we don't have gradient from forces that are inactive. 

---
Page 64
In past you had to manually specify the path. 

---
Page 66
You also optimize also for variables that prespecify a contact (foot with ground for example). You have to impose contact and dynamics consistency.

---
Page 70
When the contact variable is 1, then the limb must be touching the ground and not sliding. 

---
Page 71 
Dynamics consistency is $f=ma$ but we add a term to penalize the use of force when contact is not present.

---
Page 77
Finds quickly a solution where the robot is flying through the air. 
But later on the solver converges into a solution that first slides through the floor. Later finally the solver make the robot step to avoding the cost of sliding.


## Collocation methods for policy learning
We consider a deterministic function that maps our state into controls. 

---
Page 104
We can learn the policy from demostration, via supervised learning. The demostration can be either human or we can use our trajectory optimization to generate the data. 

---
Page 107
For each task we run the direct collocation optimization method, collect all the state and action pair. Then I am gonna fit a policy to recreate the motions, hopefully the network will generalize. 
The problem though is that this dataset can be quite difficult to fit. 

---
Page 114
The dataset might be incosistent, so we need multimodal outputs. 

---
Page 118
We want the policy to interact with the optimization policy. The additional term will couple the policy and the controls. This will force consistency between the control and the policy (Page 121).

---
Page 125
You alternate between the two problems. 