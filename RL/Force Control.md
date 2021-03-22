#lecture 
#mit6881

Two worlds:
1. multibody plan bin, box (3 dof), gripper (3 dof), the u is only 3 actuators: $M(q)\dot{v} + C(q, v)v = \tau_g(q) + Bu + \sum_i J_i^T F^{C_i}$, the state space includes both box and gripper, which are usually decoupled but with contact forces they couple with each other.
![[Screenshot 2020-11-20 at 16.13.40.png]]
2. only gripper 

The controller in the colab lab:

An inverse dynamics controller (gets to know just the gripper) tries to cancel out all the terms we don't care about. 

We can use NDI (or inverse dynamics controlller) to cancel out most of the tems and obtain a linear controller:
![[Screenshot 2020-11-20 at 16.16.36.png]]

The resulting dyanmics:
$M^g(q)\dot{v} = M^g(q)\dot{v}^{des} + \sum J^T F^C$
If no contact forces:
$\dot{v} = \dot{v}^{des}$

Now we can design a PID controller on the desired velocity given the joint coordinates:

$\dot{v}^d = k_p (q^{d} - q) + k_i \int (q^d - q) dt + k_d (0 - \dot{q})$

The diagram of the interface is
![[Screenshot 2020-11-20 at 16.24.07.png]]

Q:*How do you choose $q^{d}$ and $\dot{q}^d$ to flip up the box*? 

We are using a single finger gripper

![[Screenshot 2020-11-20 at 17.00.32.png]]

By pushing down: 
$f^1_{w_z} + f^2_{w_z} = - mg - f^g_{w_z}$ vertical balance
$f^2_{w_x} \leq \mu f^2_{\wz}$ no slip condition 
$f^1_{w_x} \leq \mu f^1_{\wz}$ no slip condition 

The last two equations imply 
$f^1_{wx} + f^2_{wx} \leq \mu (f^1_{wz} + f^2_{wz})$
which is commonly called the **friction cone**.

We have two static coefficient of frictions:
$\mu^b$: coefficnet of static friction between the box and the bin
$\mu^g$: box + gripper 

In simulation often we cheated by assigning a coefficient of friction to object and combine them to obtain the coefficient of friction between two objects. 

In walking research is while swinging the leg is often put on position track mode but just before the impact with the ground they tend to go into force control mode. 

Q:*If I apply a constant force on my firgertip on the right?*
![[Screenshot 2020-11-20 at 17.08.44.png]]

The finger will start accelerating to the right and then produce a contant force on the box, if this force is bigger than the friction coefficient this will cuase the box to slide against the wall. 

If you have a robotic foot, prescribing the contact force is a much more robust way of doing control as you are less dependent on the ground geometry. 

Q:*The more interesting part is how do we rotate the box?*

![[Screenshot 2020-11-20 at 17.19.10.png]]

At one point the finger will not be able to apply a force without slipping. 

How do we write a controller to achieve it?

We will solve the following problem

$min_{f^A_W, f^C_W}$ 
st 
$f^A_W$ in friction cone ($f^A_{W_z} \leq \mu f^{A}_{W_x}$)
$f^C_C$ in friction cone 
$f^A_{W_x} = - f^{C}_{W_x}$
$f^A_{W_z} + f^C_{W_z} \geq - mg$

For this we need ${}^W R^{C}(\theta)$, the orientation of the box can be either obtained from perception (visual) or from a contact force sensor. 

$f^C_{C_x}$ that the force we are interested in!

![[Screenshot 2020-11-20 at 17.24.47.png]]
The slop term is cause by a the vertical component $f^C_{C_z}$:
$I \ddot{\theta} = p^{cm} \times (mg) + l f^C_{C_x} + slop$

To overcome the slop we just put a PD controller
$f^{C}_{C_x} = k_p (\theta^{d} - \theta) - k_d \dot{\theta}$
The desired trajectory is handcrafted to be :
$\theta_d = 0.5 (t - 1)$

You can run the strategy on  "A force-based flip-up strategy". 

Q:*How we implement this on the kuka?*
![[Screenshot 2020-11-20 at 18.53.37.png]]

Ideal: Force sensor at the point of contact (now you can overcome modelling errors). 

Common: force/torque sensor at the wrist

You can also have a fully model base approach where you estimate the contact forces, through measurments of q, v, \dot{v}. 

If quasi-static, $v = \dot{v} = 0$, then
$\tau_g(q) + u + \sum J^T F^C = 0$

($J^T$ is the Jacobian transpose)
These are some lecture [notes](http://underactuated.mit.edu/multibody.html) about the dynamics and contact forces. 

## Indirect Force Control 
Act like a mechanical system that applys forces. 

![[Screenshot 2020-11-21 at 10.27.00.png]]

The idea is to let the gripper behave like the system above. This is called stiffness control, so you set the stiffness of the spring. 
The origianl dynamics of the point figers is:
$m \ddot{x} = u + f^c$
so we need a controller
$u = - k (x - x_0) - b \dot{x}$

"Passivity Theory" it's a way to guarantee the stability of the system. 

In some cases this is a very natural way to program a controller, by regulatingn the rest point of a spring and its stiffness. 


## Impedance Control 
In addition to stiffness and set point, you can change the effective mass of the gripper. This usually requires a large bandwidth. 


## Peg in hole insertion
Problem
![[Screenshot 2020-11-21 at 10.40.14.png]]

Being complaiant in the horizontal direction might help you align the peg with teh hole.
![[Screenshot 2020-11-21 at 10.40.42.png]]

Q:*What should be a center of compliance?*
![[Screenshot 2020-11-21 at 11.05.24.png]]

Usually this task accomplished with a passive device having a mechanical sprint (infinite bandwith!) as active stiffness control does not work very well due to limitation in sensing. The right place to put the center of compliance is a the bottom of the peg!
![[Screenshot 2020-11-21 at 11.08.42.png]]

It's super cool that you can move the cneter of compliance down into the piece!
![[Screenshot 2020-11-21 at 11.10.48.png]]

Q:*How do you do stiffness/impedance control with a manipulator*?
Key idea: **You only need to know the dynamics of the robot to control it to repond to unknown force**.

One joint: $m \ddot{x} + b \dot{x} + k(x - x_0) = f^c$

$M(q) \dot{v} + C(q, v) v = \tau_g(q) + u  + J^T f^c$

*Btw you cannot with control change the matrix M wihtout changing matrix C, because this last matrix is not unique and depends on the frame of reference*

We are gonna write a contrller that cancels out gravity and gives the right dynamics. This *joint impedance control*:
$M(q)\dot{v} + C(q, v) v + K_p (q - q_0) + K_d(v - v_0) = \tau_ext$

*the mass of the robot is configuartion dependent*

**End-effector impedance control**
Change coordinates from q to the end effector coordinates. 

The can test the precision of your model doing gravity compensation, if the precision is high the manipulator should feel like it's floating. 

Q:*How having impedance control is advantegous wrt a PD controller*?
That controller looks a lot like a PD controller. 
**It's doing PD control + gravity compensation + friction compensation**

With a PD controll you don't obtain the desired set point because of gravity. The biggest point is that you can obtain very "soft" gains and compliant robot in this way, if you try to put low gains on a PD controller the robot will be useless. In this way the robot can also deviate a bit from the desired trajectory and still accomplish the task without braking anything. 

Q:*How do we do it for the iwaa?*

Big Ideas:
- **Only knowing the robots dynamics you can program very interesting mechanical system**
- **It's good to think about forces, and you can program forces indirectly by behaving like a mechanical system**


Play around with the notebook! 