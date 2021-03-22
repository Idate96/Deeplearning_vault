After having talked to Marco last time, I am going to present a couple of research projects from which I learned a lot and  that have helped defining my research objectives. 

For each of the problems I will go through my motivation for doing it, a brief overview of the research 




Generative Neural Networks are latent models. They are composed by two networks the generator and the discriminator. The generator, $G(z)$ takes as input a latent variable z and outputs in our case an image, therefore it represent the probability distribution $p(x|z; \theta_g)$. The discriminator on the other hand has the objective of understanding if the input comes from the dataset or it's generated. It outputs a scalar value $D(G(z)) \in [0, 1]$.

The loss function for the GAN  text is:
$\begin{align}\underset{G}{\text{min}} \underset{D}{\text{max}}V(D,G) = \mathbb{E}_{x\sim p_{data}(x)}\big[logD(x)\big] + \mathbb{E}_{z\sim p_{z}(z)}\big[log(1-D(G(z)))\big]\end{align}$

In pytorch we can implement this loss via the [BCE loss](https://pytorch.org/docs/stable/generated/torch.nn.BCELoss.html) where the target label is 1 if the image is real of 0 if it's generated. 

---

In this work we explored a Bayesian formulation for the GANs for these reasons:
1. Nash equilibrium does exist for deterministic strategies 
2. Uncertainty as a regularizer (robustness)


In Bayesian GAN we replace the weights point estimates with parameterized probability distributions. At each forward pass of the network we sample the weights of the network, in our case we chose  a guassian distribution. In this case the weights are actually a latent variable of the models and we reparametrize to we can sample $\z_w \sim N(0, 1)$ where $\theta_g = \mu + z_w \sigma$.


I'll focus on the generator 
Q:*How do we train this network?*
We cannot just do a max-likelihood, maximizing the probability that the discriminator labels the picture as real, because we would have to marginalize over the latent variables of the weights
$p_{\theta}(x) = \int p_{\theta}(x|z_w) p(z_w) d z_w$
where $x = D(G(z))$ is the likelyhood that the sample $G(z)$ is come from the dataset of real images. 

Instead of that objective we are gonna maximize:
$\max \frac{1}{N} \sum_i E_{z_w \sim p(z_w|x)}[\log(p(x, z_w))]$

$$
\begin{align}
\log p(x) = \log \int p(x|w) p(w) d w \\
= \log \int \frac{q(w)}{q(w)} p(x|w) p(w) dz_w \\
= \log E_{w \sim q(w)}[p(x|w)p(w)] + H(q) \\
\geq E_{w \sim q(w)}[\log(p(x|w) + log(p(w)))] + H \\
= \mathcal{L}(q, p) + H(q)
\end{align}
$$

We can also see it in this way:
$$
D_{KL}(q(w)||p(w|x)) = D_{KL}(q(w)||p(w, x)) + log(p(x)) - H(q) = - \mathcal{L}(p, q) + log(p(x))
$$
In this case minimizing the KL divergence between the true posterior and the parameterized network is equivalent to maximizing the likelihood that the generator fools the discriminator. 

The likelyhood for a batch is 
$p(x|w) = \prod_{i = 0}^N D(G(z|w))$

The result is that the current network exibits:
- more stable training
- less prone to mode collapse 

Relevance: 
The problem of overfitting is aggravated in Deep-RL, if the env NN overfits the policy might exploit, leading to changes in the policy that degrade the performance at their own time. 
[[ME-TRPO]] results
Using uncertainty as surrogate loss 

---
Thesis work: 
I am going to present the work done in collaboration with NASA. 
The objective of the research project is to model how human adapt their control strategy while controlling time-varying dynamical systems. 
The main characteristics that this controller must have are: 
- a represtnation of the controlled system 
- time-varying gain 
- albe to scale up to high dimensional spaces 

We designed two scenarios ot test the controller the pilots in the simulator:
1. will to trajectory following under load disturbances
2. reach an end state 

For this project we considered two main optimization techniques:
- MRAC: lyapunov based 
- iLQR: model based optimization (shooting method)










---
PhD interests:
- Model based RL: create complex representation of hte world, also directly from pixels 
- Offline RL: can we 
