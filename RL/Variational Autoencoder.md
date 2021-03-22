The variational autoencoder is composed by two networks: 
1. Encoder: $q_{\phi}(z|x) = \mathcal{N}(\mu_{\phi}(x), \sigma_{\phi}(x))$
2. Decoder: $p_{\theta}(x|z) = \mathcal{N}(\mu_{\theta}(z), \sigma_{\theta}(z))$

In a latent variable model is common to have a simple distributions over z (p(z)) and x given z (p(x|z)). But when you integrate out z you can obtain a very complex distribution. The prior is usually N(0, 1). 

![[Screenshot 2020-11-18 at 09.40.39.png]]

The objetive for training the autoencoder is to maximize the [[Variational Lower Bound]]:

$\mathcal{L}_i = E_{z \sim q_{\phi}(z|x_i)}[\log p_{\theta}(x_i|z)] - D_{KL}(q_{\phi}(z|x_i)||p(z))$

The first term makes us reconstruct the image accurately and the second term says that encoding of an image should be close to the prior. 