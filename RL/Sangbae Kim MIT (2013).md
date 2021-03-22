[[SLIP model]] for running is an example of biological inspired principle. 

What is stickiness?
![[Screenshot 2020-11-02 at 19.37.20.png]]
The material used there is non-sticky material, the stickiness is a property of the system. 
![[Screenshot 2020-11-02 at 19.37.49.png]]

## Efficiency in locomotion
![[Screenshot 2020-11-02 at 19.44.31.png]]
In locomotion the measure is different is not well defined. What is the work output?
Locomotion is energy dissipation, so efficiency is actually 0!

The total cost of trasport is only an approximate way to measure it (dot product between gravity and speed is 0!)
Q:*Where should we focus to improve the efficiency?*
Q:*Where is the energy loss?*
Battery is neglectable, most energy loss is through actuators (our muscles 20% efficient) and trasmission (one of the biggest problems in robotics). Interaction with the ground also causes losses (friction, impact loss (vibrations), drag).

Q:*How do we minimize the loss?*

## Structure 
Are leg made of special materials?
Tendons and muscles are 4 times weaker than aluminum and 20 times weaker than kevlar (we also have better materials). 

![[Screenshot 2020-11-02 at 19.52.01.png]]

With a normal foot you are gonna have a lot of bending stress on the foot. But a human is very different. There are many bones on the feet (they cannot take bending moment because they are separate). Bones take the compression forces and the moment get counteracted by tension in the tendons. 
![[Screenshot 2020-11-02 at 19.53.09.png]]

With tendons you can minimize the stress concetractino and decoumple the bending moment into compression and tension.
![[Screenshot 2020-11-02 at 19.55.25.png]]

Animals might minimize bending stresses on the bones. 
We also tried to minimize the body inertia. 

By putting together the motor and the gear box we can put the center of mass veyr high. 
![[Screenshot 2020-11-02 at 19.56.17.png]]

Also we use the spine when legs moving coupled toghether (gallopping) and not when not couples (trotting)
![[Screenshot 2020-11-02 at 19.57.29.png]]

## Actuator 
Q:*What characteristic do we need for a good actuator?*
-High mass specefic force/torque
-High mass specific power
-Impedance control (stiffness damping variability)
-High bandiwith (fast actuator)
-Efficiency
-Soft structure 

### Power density 
Muscle ~ 0.08 kW/kg

If you a commercial motor you can find motors ~0.3 kW/kg
![[Screenshot 2020-11-02 at 20.01.29.png]]
So there are two orders of magnitude between electromechanical actuator and our muscles. So what is the problem??

One simple answer is that those powers are available only at very high speed and low torque.
Our quad for example can hold 8 times my body weight when i crouch, this mean that the moment arm is very small. It is very small to allow to have a large range of motion! So our muscles have high force!

Q:*Why don't we use a gear reduction system?*
**That's where all the trouble happens**

In the picture you can see the torque-voltage diagram for a motor. Why don't you go outside the red area (like is constant voltage)? Going to the right is possible and there is nothing stopping you (maybe heat, don't burn motor). 
Going up, how much can you go out? if you put to much current you might start demagnetizing your magnetic field -> saturation torque. 
Most companies don't put the saturation torque. Now you have a much bigger space to play with because you don't need **Continous torque**, while running or walking the torque is periodic. 

For the cockroach robot i used 32 V on a 6 V motor. It's all about heat (discontinous use)!

Q:*What is the most important measure for high torque?*
we want $\max \frac{\tau}{m}$. You always win making them larger and thinner but for manufacturing reason often we have the opposite.
![[Screenshot 2020-11-02 at 20.11.19.png]]

So we propose a new paradigm:  
![[Screenshot 2020-11-02 at 20.15.25.png]]
 
 Impacts travel at the speed of sound in steel so with the geared motor even with a high bandiwith motor you will never respond in time to impacts. 
 
 Very popular are series elastic actuators, because it can handle impact and you can measure force through the spring. 
 
 So we propose this new design of motor with no springs. So stiffness and high damping, but you can increase the stiffness to extremely high levels (like a rock!). So stiffness can be changed!
  ![[Screenshot 2020-11-02 at 20.19.14.png]]

Custom design of motors:
![[Screenshot 2020-11-02 at 20.20.42.png]]

With low impedance you can also recover power, high damping is not dissipating!
![[Screenshot 2020-11-02 at 20.21.57.png]]

Peak power is 3kW, average power is 1kw (~100 W per motor). Most of energy goes into heat!
![[Screenshot 2020-11-02 at 20.24.10.png]]

### Cost of tranport  
Decrease of cost of transport with higher speed (ok). But how come that total power decrease going faster??

with an inverted pendulum model, you can find the speed at which the centrifugal force is equivalent to the gravitational force. This means you cannot stay attached to the ground -> you cannot walk, then you run!
Animals dont like to be around 1m/s, too fast to walk too slow for run. 
 ![[Screenshot 2020-11-02 at 20.26.18.png]]
 
 Horse (harvard bio-mechanical):
 ![[Screenshot 2020-11-02 at 20.29.24.png]]
 
 So for the cheeta robot, maybe the transition trotting speed is higher than 6 (horses alrady gallop). 
 
 When muscles are streched, force is very low, they they get stronger, but closer to the end of the range of motion they get weaker. Force depends on the position, also if you move fast the force drops significantly. 
 
But electric motors are indipendent of position and indipendent from velocity!!

Different animals efficiency (best fishes): 
![[Screenshot 2020-11-02 at 20.33.09.png]]
![[Screenshot 2020-11-02 at 20.34.01.png]]

We could achieve really nice efficient locomotion!

(Power is not a really good measure)

![[Screenshot 2020-11-02 at 20.36.21.png]]

### Control 
1. first 2d locomotion then couple with direction to go 3d
2. How do you how much force you need to run? 

If you integrate the normal force (of a step) over time, that should be the same as $mg\Delta T$ because your vertical momentum is conserved. If I know my speed, how long does it take to return my leg, I know the duty cicle of my leg (percentage of time spent on the ground). I know how much impulse i need from that step, if i fix the swing time (assumption). 

![[Screenshot 2020-11-02 at 20.42.11.png]]

We optimize for the limit cycle at 3 m/s. Now you need to make it stable. 
(there are my types of gallop)

If the leg is moving at 100 hz, your compute cycles should be much much shorter. 

Q: *How is the chicken stabilization possible?*
They have an additional IMU in the pelvis. What if I put two imus on the cheeta and stabilize the head?