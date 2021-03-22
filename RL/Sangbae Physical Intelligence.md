For cheeta they use Regularized MPC

Trajectory for the robot comes from humans or vision
![[Screenshot 2020-11-03 at 09.03.35.png]]
The MPC controller have a 0.5 secodn control horizon. The whole dynamics are not used to speed up the convergence of the algorithm (100-40 hz). 

Q:*What is the cost function?*
There is not science on how to tune the cost function, but no one talks about it. Minimizing velocity and position is not enough. 

The heuristic model is imporoving.
![[Screenshot 2020-11-03 at 09.09.10.png]]
We want the robot to do so many tasks that is very hard to choose a cost function. In safe driving is very hard to write a cost function. 

 Force of inpact is also caused by the kinetic energy of the body. Commanded force in blue but the actual force is much higher. We want to minimize yellow (you move very slow or make the mass very small).  
 
 ![[Screenshot 2020-11-03 at 09.14.49.png]]
 
 Limb inertia is not hte only source of inertia, most of the inertia is rotor inertia. You need to include it if you want to simulate forces.  
 
 ![[Screenshot 2020-11-03 at 09.16.42.png]]
 
 ### impact mitication factor 
 Operation space is the foot space, what iteracts with the environment. 
 this is the inertia that I need to investigate when understanding how much impact the machine can handle. 
 You need  a very large force bandiwht (contact time is 80 ms)
 
 ![[Screenshot 2020-11-03 at 09.18.31.png]]
 
 ### Actuators 
 Direct drive arms (very high bandwitdth) are not often used to interact with the environment. These arms you don't ahve a lot of torque density. 
 
Series-Elastic are also very popular. Idea: let the spring handle the impact. The impact location does not "see" the inertia. Lose control bandwith and complex. You cna mitigate the bandwith with a very stiff spring. Hydraulics is the best against impact, it does not mitigate the impact but it does not brake. The forces are transmitted through the liquid. 
The efficiency of hydraulics is terrible though!

![[Screenshot 2020-11-03 at 10.02.10.png]]

Timeline:
![[Screenshot 2020-11-03 at 10.05.23.png]]

In high impact the friction will show as inertia (what does it mean??)
