Type: #paper 
Citationkey: 

# Summary 
If you do data  for reinforcement learning you do much better.
*claim*: this as good of a gain as the one you get from the last year of research!



# Analysis 

## Augmentation
Types of augmentation:
- Crop
- Grayscale
- Cutout
- Cutout color
- Flip 
- Rotate
- Random Conv with a color filter 
- Color jitter

The augmentation (RAD) must be applied consistently: the augmentation for the policy and value function must be the same (*why?*)


## Experiment 

Crop appens to be the best data augmentation method. 

[[Spacial attention]] map for the walker shows that crop puts the attention of network on the physical agent. 

They also investigate if the RAD is able to win the on procedurally generated levels on OpenAI ProcGen. There is evidence that the effects of the augmentation are *interaction effects*, for example in Jumper actually hurts the agent performance. 



## Critisism 
It is state of the art done on limited data: the [[DMControl]] benchmarks. 
What happens if you do [[Dreamer]] + [[Data Augmentation]]? Is it an orthogonal improvement to the other methods? 
*Suspicions*: data augmentation could work better because all the images are simulated in the real world we could expect less gains. 


# General Thoughts
Usually if you a database, say of images, you used to go over your dataset multiple times. It turned out to be more successful to do [[Data Augmentation]]. 



