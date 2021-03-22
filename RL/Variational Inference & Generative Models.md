#lecture 
#cs285 

## Probabilistic Latent Variable Models

Page 3
Probabilistic models represent probability distributions. 

For example conditional models $p(y|x)$, in this case you don't care about the distribution over x, but you care about the distribution of y given x. 

Policies are an example of conditional models (over the states)

---
Page 4
[[Latent Variable Models]]

---
Page 6
Some example of latent variable models.

In the following case we are actually modelling $p(y|x) = \sum_z p(y|z, x) p(z)$ because the latent variable is given as input to the network. 
![[Screenshot 2020-11-16 at 20.37.01.png]]

Latent variable model come up in model based RL: model based RL with images ($o_t$). 

![[Screenshot 2020-11-16 at 21.07.06.png]]

---
Page 7
We can use variational inference and reinforcement learning to model human behaviour. Can we infer their thought processes and reward functions?


A [[Generative model]] is a model that generates x. Not all generative models are latent variable models and not all latent variable models are generative models. But usually generative models are modelled as latent variable models. Generative models have to represent very complex probability distributions and it's easier to represent a complex probability as a product of simple distributions. 


---
Page 8
Q: *How do we train the model?*
Usually we use max-likelihood say for $p_{\theta}(x)$.
The problem arises if $p(x) = \int p(x|z) p(z) dz$  when 
![[Screenshot 2020-11-16 at 21.16.22.png]]
becomes possible only in very special case, for example $p(x|z)$ is a gaussian mixture model (still not very good due to poor numerical properties of optimization landscape).

---
Page 9
Q: *How can we estimate the log likelihood and its gradient in a tractable way?*
This is a reasonable objective: the expected log-likelihood. 

Intuitively amount to guess the latent variables. You can think about the latent variable as partial observation of the data (made by both x and z). You only observe the x, and guess what the z were (guess the cluster) and construct a fake label z. You now can do maximum likelyhood on $p(x, z)$. In reality you don't know the exact z, so instead of taking the most likely one, you take entire distribution and average the likelihood weighted by the probability of that z: $E_{z ~ p(z|x_i)}[\log p_{\theta} (x_i, z)]$.

So the objective given N observations of x is :
$\theta = \arg, \max_{\theta} \frac{1}{N} \sum_i E_{z p(z|x_i)}[log_{\theta}(x_i, z)]$

Q:*Why is this objective more tractable?*
Expected values can be estimated with samples, so you can sample from $p(z|x_i)$ and than sum to get the log probabilities of those samples (x and z).

You can't do the trick on the previous slide because the log of an integral does not decompose linearly. 

The objective now is to find: **the posterior p(z|x_i), which will allow us to optimize for the likelihood**. We are gonna use variational inference to estimate the posterior probability of latent variables z given observation x. 


## Variational Inference
Assume we approximate the posterior $p(z|x_i)$ with a gaussian distribution $q_i(z) = N(\mu_i, \sigma_i)$, specific for the point $x_i$

This is the original distribution 
![[Screenshot 2020-11-17 at 16.02.44.png]]
and this is the approximation with only one peak
![[Screenshot 2020-11-17 at 16.03.31.png]]

We can contruct a lower bound on the log probability of $x_i$. This is great because maximizing this lower bound will push up the log probability of $x_i$ (if the bound if thight). 

$\log p(x_i) = \log \int_z p(x_i, z) = \log \int_z p(x_i|z) p(z) = \log \int_z p(x_i|z) p(z) \frac{q_i(z)}{q_i(z)} = \log E_{z q_i(z)} [\frac{p(x_i|z)p(z)}{q_i(z)}]$

---
Page 12

Now we apply [[Jensen's Inequality]]:
$\log p(x_i) =log E_{z \sim q_i(z)} [\frac{p(x_i|z)p(z)}{q_i(z)}] \geq E_{z \sim q_i(z)} [\log \frac{p(x_i|z)p(z)}{q_i(z)}])  =  E_{z \sim q_i(z)} (\log p(x_i|z) + log(z)] -  E_{z \sim q_i(z)} [log(q_i(z))]$

The nice thing about this is that everything is tractable!

$\log p(x_i) \geq  E_{z \sim q_i(z)} (\log p(x_i|z) + \log(z)] + H(q_i)$
where we substituted the last term for the [[Entropy]].

To maximize the first term we can select:
![[Screenshot 2020-11-17 at 17.08.27.png]]
but if we want to maximize the entropy, we have to spred out the distribution while still remaining in regions where $p(x_i, z)$ is large.

---
Page 15
A good $q_i(z)$ approximate $p(z|x_i)$, in the sense of minimizing the KL divergenge between the two distributions $D_{KL}(q_i(z)|p(z|x))$.

Q:*Why?*
$\log p(x_i) = D_{KL}(q_{i}(x_i)||p(z|x_i)) + L_{i}(p, q_i)$
where $L_{i}(p, q_i)$ is the [[Variational Lower Bound]],  since the KL diverenge is always positive
$\log p(x_i) \geq L_{i}(p, q_i)$
minimizing the divergence also provides a thight bound!

---
Page 16
Maximizing the lower bound wrt q minimizes the KL divergence.

---
Page 18
We have a separate $q_i$ for each datapoint, which is way too high.

To solve this problem what is we train a network ($q_i(z) = q(z|x_i)$) that outputs the correct probability distribution given our input?

$q_i(z) = q(z|x_i) \approx p(z|x_i)$

In our generative formulation we a neural network that maps z to x and one that maps x to z

![[Screenshot 2020-11-17 at 17.44.52.png]]

## Amortized Variational Inference 
Page 20

You can take the gradients of  $p_{\theta}(x_i|z)$ because it's a network that you train that takes z as input and outputs x. z is sampled from our parametrized distribution $q_i(z) = p(z|x_i)$, which is also a network.
The first network is trained by maximizing the variational lower bound. 

---
Page 21 
Training procedure for amortized VI.

Q: *how do we take the gradient of the VI lower bound wrt to $\phi$*?

The gradient (from policy gradient) (usually more than 1 sample) tends to suffer from high variance. 

---
Page 23

This trick is not usually available in RL. In RL we use policy gradients because we cannot compute gradients through the dynamics. 
This problem is not present here. 
We normalize the gaussian distribution as  $z = \mu_{\phi}(x) + \epsilon \sigma_{\phi}(x)$, with $\epsilon \sim N(0, 1)$. So now we just sample from N(0, 1) to estimate the gradient.

---
Page 24
Lower bound expression. 

Q:*What is the purpose of this?*

---
Page 25
Derive the formulas yourself.


## Example
Page 27
[[Variational Autoencoder]]

---
Page 29 
[[Conditional Models]]

Here you can show the architecture for a multi modal, imitiation learning. 



