#cs287 
#lecture 

In this lecture we focus on the formulation of optimal control problems

---
Page 2
In [[Shooting methods]] you directly optimize over the control while in collocation you optimize for both state and action at the same time. 

Now we focus on the open loop control:

For shooting we eliminate the x variables and express them in terms of the dynamics and actions 
![[Screenshot 2020-11-19 at 16.04.27.png]]

For [[Collocation methods]] we keep the x around 
![[Screenshot 2020-11-19 at 16.05.14.png]]

The optimal solution for both problems is the same, so why do you care about different formulation?

In optimization is very important that the problem is well conditioned. 
In shooting methods the $u_0$ appears everywhere, so the problem is very poorly conditioned since the choise of $u_0$ can have huge consequences. 
On the other hand if you have a very large state space you might not be able to afford the collocation method. 
The collocation method nicely decouples, $u_0$ only affects the first step and does appear anywhere. 

Let's now look a feedback controller:
Assuming we have a policy $\pi_{\theta}(x)$

The shooting method 
![[Screenshot 2020-11-19 at 16.09.27.png]]

The collocation 
![[Screenshot 2020-11-19 at 16.10.36.png]]
you can also collocate even more by keeping the u variable even more because the form of $\pi_{\theta}$ might constrain you too much. 
![[Screenshot 2020-11-19 at 16.12.00.png]]

---
Page 3
During rollout just use the whole sequence or you can do [[MPC]]. 

---
Page 5
In open loop shooting, you can do shared calculation and use dynamic programming to minimize computation cost.

Another thing that is tricky, you find your current control then you roll out the policy, then you can do backpropagation get the gradient and update. But during the rollout you could have numerical instabilities in the rollout. To roll out the u, you can have mpc in the inner loop to stabilitize it. 
It's not very clear how to initialize, in the shooting method. In the collocation method you could draw the initial trajectory as well. 

The main downsize of a collocation method is that it might converge in a local minimum that is not feasable. You might have to reinitialize and start again. 
In collocation you often have MPC has well because you might have noise in the execution. 

Q:*What about iLQR?*
We return a feedback linear policy with a shooting method. You rollout the policy and backpropagate with value iteration. You can solve the same problem with a Newton's method applied to the shooting problem but it's not gonna be nearly as efficient becasue it's not taking advantage of the structure of the problem. 

If you want a non linear controller  like a NN, you might have to apply Newton's method, or first run iLQR for many different condition and do behavioural cloning with a network. 