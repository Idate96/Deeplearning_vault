A system is t-time step controllable if at any start state $x_0$ we can reach any target state at time t. 
![[Screenshot 2020-10-30 at 16.05.03.png]]
the abouve system of linear equation in $u_1, ..., u_{t-1}$ has a solution for all choices of $x_0$ and $x_t$ if 
if $rank[A^{t-1}B A^{t -2}B, ..., AB, B] = n$ then system is controllable. 
where n is the dimension of the state space. 

Using the [[Cayley-Hamilton theorem]], adding matrices $A^{t}, A^{t + 1}}$ you cannot increase the rank