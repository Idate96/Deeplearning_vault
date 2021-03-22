![[Screenshot 2020-11-07 at 11.11.44.png]]

Train a single a model that is as good as the ensanble. You use the ensable to output the probabilities over a set of outputs and then you can use these as labels (soft because we using probabilties)

[[Guided policy search]] also does a distillation process. 

Here distillation is not trying to get a smater model and encapsulating it into a smaller model. Rather it's taking heterogenous models and synthesize into a single model 

## Distillation for multi task transfer 

![[Screenshot 2020-11-07 at 11.34.29.png]]

The expert is gonna run supervised learning for action sampled from the local policies. 