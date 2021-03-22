These are probabilistic models, where there are some variables other than the evidence and the query. For example in $p(y|x)$ x is the evidence and y is the query and in $p(x)$ x is the query. 

If you have extra variables in your models, those need to be integrate out to obtain the probability of the query variable for example. 

A classic example a **mixture model**. Given few clusters of points as the one you see below, it's very convenient to capture the distribution of points with a mixture of gaussians.

![[Screenshot 2020-11-16 at 20.05.42.png]]

In this case the latent variable $z$ is a categorical variable (0, 1, 2), which identifies to which cluster belongs to. **$x$ is a 2 dimensional variable** that gives the location of the points.

The same can be done for conditional models. 
For example if we have a [[Mixture Density Network]], encountered when we were doing [[Supervised Learning of Behaviours (2)]] ([[Imitation Learning]]), used to model multimodal situation like avoiding a tree!

![[Screenshot 2020-11-16 at 20.10.11.png]]

*Example*:
Suppose we want the probability distribution of taking a multimodal action $y$ given an input $x$. So we design the network in the image that outputs $w_i$, $\mu_i$, $\Sigma_i$, which are the probability that the caterorical variable is i, and the mean and standard deviation of gaussian distribution of y given the categorical variable. 

![[Screenshot 2020-11-16 at 20.16.48.png]]

Q: What probability distribution are we representing? 
The input of the network is $x$, therefore everything that is output must be conditioned on that. This means that with $\mu_i$ and $\Sigma_i$ we are representing $p(y|z_i, y)$ (or $p(y|z, y)$ in brief). 
With $w_i$ we are representing the probability of that $z = i$, **given** the input x, therefore $p(z|x)$.

The total model is $p(y|x) = \sum_z p(y|z, x) p(z|x)$

---
In general we have a distribution $p(x)$ (complicated), we also have a prior over $z$, usually a **very simple** distribution. 
Then we may represent the mapping from z to x, $p(x|z) = N(\mu_{nn}(z), \sigma_{nn}(z))$, with a simple  conditional distribution (in this case a gaussian).  Now the functions for the mean and the variance can be very complicated (like a neural network) but the actual distribution is simple.

Summary:
- p(x) is not simple because we cannot find a parametrization for it
- p(z) is simple because a simple parametrization like a gaussian capture it perfectly 
- p(x|z) is simple because you can represent it with a gaussian distribution even if its parameters can be very complex functions of z

![[Screenshot 2020-11-16 at 20.28.35.png]]

The product of the conditional and prior distribution can be very complex though:
$p(x) = \int p(x|z) p(z) dz$

Very powerful idea:
**We can model very complex distribution with simpler ones that we can parametrize**
