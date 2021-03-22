#cs287 
#lecture 

Data:
- Large scale robotics data collection 
- Efficient reinforcement learning
- Unsupervised RL 


## Why hard to use sim data?

Neural networks overfit tiny different in data distribution, but errors tend to compound. 

## System ID
How to improve the simulation with real data. Minimize the loss that computes the difference between the behaviour of the robot in the real world and in simulation. 

## Domain Adaptation 
Supervised domain adaptation. 


Unsupervised domain adaptation. 
- Weakly supervised
- Self supervised
- Unsupervised: take image to image translation model. You have feed the sim data to the translated data, that tries to make it more realistic

## Domain Randomization 

Take a basic set of features that are really necessary to solve the task. All the inconsequencial objects in the scene instead can be randomized. 

[[CAD2RL]] is an interesting paper. 

---
*Page 71*
Domain randomization for vision: pose estimation. 
A lot of different randomization.

---
*Page 100*
Intuitiion:
- if you have an unrandomized sim data, you are covering a small subset of the training data. Why don't we expand the space covered by the distribution of randomized data. As you make the distribution of parameter wider the performance should improve. 
- randomize features that you want your model to ignore
- Meta learning: one rolllout in an environment is a task, the reccurent state of the policy allows you to adapt for the current scene. 

## Extension 
[[SimOpt]] automatically incorporate real-world data to world design. 
[[Learning to Simulate]] 
[[Active Domain Randomization]] 
[[ADR]]