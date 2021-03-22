Type: #paper 

# Summary


# Analysis
> Let us consider a
shallower architecture and its deeper counterpart that adds
more layers onto it. There exists a solution by construction
to the deeper model: the added layers are identity mapping,
and the other layers are copied from the learned shallower
model. The existence of this constructed solution indicates
that a deeper model should produce no higher training error
than its shallower counterpart.

Q:*Why deeper models don't learn the identify*?
The intialization usually sets the weights of the network pretty close to zero in a random fashion. Learning the identity function is a difficult function to learn. 

**Key Idea**: can we do something to make the default function that is learned the identity function? 

This brings us to *residual connection* $H(x) = F(x)  + x$
![[reslayer.png]]
The linear layers (who tend towards the zero function) now learn *differences*. Therefore the default output of the layer is the identity function approximately. 

By chaining this blocks you get a *residual network*.



## Complexity 
The residual network allow to go deeper by restructuring the layers and doing less computation (19 vs 4 TFLOP per forward pass). In the paper the resnet layers have a lower number of filters. 

### Bottleneck blocks
You can save computational power by first projecting down the dimension of the vector. 
![[bottleneck.png]]


## Experiment
The paper compares VGG with a 34-layer plain and 34-layer residual. 

The training error for the 34-layer network is higher than for the 19 layer one but the resnet is lower. The residuals connection mostly help the deeper you go. 
![[resnet_exp.png]]

They also build 50, 101, 152 layer resnet, which performed the best. Then they built an ensamble of them. 

They were crazy and they built a 1202 resnet, even though it performs worse than the 110 network. 

Q:*Are we gonna run into the same problem again*?
No, the training error is for both at 0, the resnet 1202 is overfitting. Today we use a lof of [[data augmentation]] so overfitting was still a thing back then. 

## General Thoughts
Before this paper Imagenet scores got better with always deeper models. These models though are harder to train (*why?*). 


### Applications
Res50 is still used for almost every segmentation task 