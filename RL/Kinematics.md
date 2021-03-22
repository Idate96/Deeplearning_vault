# Robot Kinematics

For a more in depth explanation check it out here at [MIT](http://manipulation.csail.mit.edu/pick.html)

## Notation

$P^{A}$ = position of A (A is a point, ...) but always relative to something

${}^BP^{A}$ = position of A relative to B

if ${}^BP^{A} \in \mathcal{R}^3$ then we need a reference frame so 

${}^BP^{A}_{C}$ = ... in frame C

If I wanted to change the coordinate system in D that is just a **translation** from C then  ${}^BP^{A}_{D} = {}^BP^{A}_{C}$. 

 ###  Algebra
 
Position adds: ${}^BP^{A}_{C} +  {}^AP^{E}_{C} = {}^BP^{E}_{C}$

Additive inverse $- {}^BP^{A}_{C}  = {}^A P^B_C$

The right and left superscript are usually points but can be frames, where by that we mean the their origin. 

Frames have also an orientation 
${}^B R^A$ = orientation of A in B

Rotation multiply
${}^B R^A  {}^A R^C = {}^B R^C$

Multiplicative inverse
$({}^B R^A)^{-1} = {}^A R^B$

Representations for rotations:
- [[Rotation matrix]]
- [[Quaternion]]
- [[Axis angle]]
- [[Euler angles]]

There is no one representation that rule them all. For different problem you will pick a different representation. 

A **Pose** is the position and orientation of a frame. 
${}^B X^{A}$ = pose of A in B (both are frames)

Rigid transform: 
- Translate 
- Rotate 

Poses multiply and have an inverse:

${}^B X^A  {}^A X^C = {}^B X^C$
$({}^B X^A)^{-1} = {}^A X^B$

${}^A P^{B}_C = {}^C R^D {}^A P^B_D$

There is an important frame $W$, the world frame
$R^D = {}^W R^D$
$X^D = {}^W X^D$
$P^B = {}^{W}P^B_W$
${}^A P^B = {}^A P^B_A$

Btw the colors for the axis are $XYZ = RGB$.

## Forward kinematics 
Mapping from the joint angle to the pose of a particular body. 
$X^{B} = f^B_{kin}(q)$

Inverse kinematics instead map from pose to joint angles, the solution of this problem is not unique, there are multiple joints position that will lead to the same position of the end effector. 

Differential kinematics $\nabla_q f$

### Joints 

*Joint*: adds contraints between two bodies. When you add a joint you are taking away degrees of freedom.

*Revolute Joint*: given two bodies, linked with a revolute joint, their poses, $X^P, X^C$, are be related by 

${}^PX^C(q) = {}^P X^M {}^M X^F(q) {}^F X^C$


We have a **Kinematic Tree** that relates all the different bodies that make up a manipulator (often traversed from the leaf to the root).
>$X^B$ = plant.EvalBodyPoseInWorld(body, context)

Here you can find an example of [forward kinematics](https://www.user.tu-berlin.de/mtoussai/teaching/14-Robotics/02-kinematics.pdf) for a serial robot.

## Differential kinematics 
The objective is to understand how changes in q relates to changes in $X^G$. 

$\frac{d}{dt} X^G = \frac{\partial f^G_{kin}(q)}{\partial q} \frac{q}{dt} = J^G(q) \frac{dq}{dt}$

The key idea:
1. For orientations, there are many representations but no one to rule them all. For rate of change of ratation, you can pick one and be happy for ever: angular velocities. 

### Rotational representations
- 3x3  rotation matrices. Composition is just matrix multiplication (which is good if you are doing it on a GPU)
- Unit quaternions (4 numbers in SO(3))
- Euler angles (3 numbers in $R^3$)
- [Axis angle](https://en.wikipedia.org/wiki/Axis–angle_representation) (3 numbers in $R^3$: 2 numbers to define the unit axis direction (two angles of polar coordinates) and 1 number for the angle magniture)

### Time derivative of rotation 

Angular velocity, where the magnitude is the rate of change and the direction is the axis (x, y, z)
$$
\begin{bmatrix}
\dot{\theta} x \\
\dot{\theta} y \\
\dot{\theta} z 
\end{bmatrix}
$$

It is better because it's just three numbers. 
Q:*why can we get away with three number for rates but not for rotations?*
In rotations both the axis and euler angles have singularities that are resolved for example with quaternions. A classical example is a gimbal lock. 
The reason why it happens is because $\theta$ wraps around $2\pi$ while the rate are unbounded. 

In 2D we are totally happy just having a single $\theta$, why is this ok?
For any $\theta$ there is a unique rotation. And for any rotation there is a unique $\theta$. 

In 3D for any axis angle we can get a unique rotation. But for any rotation there not a nice mapping back to a unique axis angle. 

What if we use $sin(\theta)$ to resolve the problem of the wrapping, the sin will naturally wrap. But we need to carry along as well the cosine to reconstruct the actual angle.

$$
\begin{bmatrix}
\sin(\theta) x \\
\sin(\theta) y \\
\sin(\theta) z \\
\cos(\theta)
\end{bmatrix}
$$

It turns out that is actually the definition of quaternions. 

 ## Angular velocity
 
${}^{B} \omega^A_C \in \mathcal{R}^3$ = angular velocity of frame A wrt frame B expressed in frame C

Summation(not obvious but it works out):
${}^{C} \omega^A_F = {}^{C} \omega^B_F + {}^{B} \omega^A_F$

Change of coordinates:
${}^B \omega^A_G = {}^G R^F {}^B \omega^{A}_F$ if ${}^G \dot{R}^F = 0$

${}^B v^A_C = {}^B\dot{P}^A_C$ = translational velocities

${}^B \mathcal{V}^A_C = [{}^B \omega^A_C; {}^B v^A_C] \in \mathcal{R}^6$ = spacial velocity

---
The original goal was to find 

$\frac{d}{dt} X^G = \frac{\partial f^G_{kin}(q)}{\partial q} \frac{q}{dt} = J^G(q) \frac{dq}{dt}$

$V^G = J^G(q) \dot{q}$ 

What's in $\dot{q}$?

For the iwaa robot $q \in \mathcal{R}^7$ so $\dot{q} \in \mathcal{R}^7$

For a brick, $q \in \mathcal{R}^7$, 4 dof for the orientation (quaterions) and three for the position. 

---
### Aside on degrees of freedom 
Let's suppose you have a two link robot arm in 3D with revolute joints (and a fixed base).
The two bodies pose originally has 12 degrees of freedom ((3 + 3) x 2), but each joint introduces 5 constraints (3 position and 2 rotation) the end result is that the arm has only two degrees of freedom (12 - 10 = 2)

---
$X^{0} = f^{0}_{kin}(q_0)$, if q is a quaterion description the pose $X^{0}$ is a 3x4 rigid transform. 

$\dot{q}$ is 7 elements, $v$ is actually 6 elements spacial velocity. 

q - generalized position (position + rotations)
v - generalized velocity 

$V^G = J^G(q) \dot{q}$ or $V^G = J^G(q) v$
>calcJacobianspacialVelocity(..., wrt q or wrt v)

## Jacobian based control 
I am in some configuration q, what command can I send to the joint to reach a desired $V^{G_{des}}$?

Can I use 
$V^G = J^G(q)\dot{q}$
and invert the jacobian to obtain a desired joint velocity
$\dot{q}_c = J^G(q)^{-1} V^{G_des}$

Q:*Is the jacobian invertible?*
For the iwaa the Jacobian is a $6 \times 7$ matrix. 
Since given a matrix $m \times n$ if $m > n$ then there are an infinite number of solutions we still have a chance. 

Q:*Can I find a pseudo-inverse that will allow  me to find a feasable q_{c}*?

$\dot{q}_c = (J^G(q))^{+} V^{G_{des}}$
This pseudo inverse gives the correct $\dot{q}_c$ and among the feasable solutions picks the one with minimum norm. If there is not solution it will give the one that is closest as possible (in the least square sense).

Q:*Does (J^G(q))^{+}  returns an exact solution*?

$J^{G}(q)(J^G(q))^{+} v^{G_{des}} = v^{G_{des}}$

Does not given an exact solution when the $rank(J^G(q)) < 6$. In practice this happens in few points and your robots blows up before reaching that point. You can mostly about the conditioning of the matrix in particular about the singular values that should not be too small. 

You can create a controller for iwaa in this way just by integrating the desired joint rates. 

This solution thogh does handle contraints on the joint velocities, so next time we will look on how to obtain the pseudo-inverse jabocian using contrained least squares. 

#todo add next lecture notes 














