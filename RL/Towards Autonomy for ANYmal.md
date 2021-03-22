#talk 

Applications:
- Search and rescue
- Underground
- Sewer and mine inspection
- Forest mapping 
- Industrial inspection

## Planning and control for robots
You have a high reference command coming from the user or a high level planner, motion planner that generates motions, a tracker that generate joint level commands. 

Planner: planner that can be executed very fast. Now we reached a point where we can full body motions, and joint optimization of footholds (using full body dynamics).

Tracker: inverse dynamics with some hierachical objectives

![[Screenshot 2020-11-04 at 15.07.53.png]]

The effects of limited actuator bandwith are not usually modelled in the system. Last year Ruben with Frequency-aware MPC showed how to shape a cost fucntion in the frequency domain to cope with that. 

The tracking controller runs at more than 100 hz, the planning controller is much slower. To cope with this issue, we also look at the feedback behaviour $k x$. 

---
We need a very fast update rate of the environment: fast elevation mapping on GPU. The it should be of comparable speed compared to the planner. 

(Impedance control stiffens the body of the robot).

## Data-driven approaches 
These different environments are impossible to model in simulation and hard to sense from perception. 

We took a similiar control architecture, we trained a model following this scheme where we force the latent represenation of the student and the teacher to be the same:

![[Screenshot 2020-11-04 at 15.27.13.png]]

We gradually trained it on a adaptive environment curriculumn:
![[Screenshot 2020-11-04 at 15.27.28.png]]

## In between 
Pure optimization takes a lot of online time, while RL policies takes a lot of offline time to train but not much compute while deployed. 

Can we combine the advantages?
MPC-net (Carius)
Use the optimal control as a demostrator
This solution minimizes an Hamiltonian (which encodes contraints), ten this Hamiltonian is used to as loss function in RL.

![[Screenshot 2020-11-04 at 15.32.30.png]]

## Trends

Perception

Planning

Control

Maybe we should focus more on perception??

### Limitation

Legged robots usually looks at 2.5D information. 

![[Screenshot 2020-11-04 at 16.03.36.png]]

We take a lot of info and feed it to a NN to generate a distribution of features. We choose the feature that have a hihg likelyhood to be safe. 