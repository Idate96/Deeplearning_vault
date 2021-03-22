Generative Neural Networks are latent models. They are composed by two networks the generator and the discriminator. The generator, $G(z)$ takes as input a latent variable z and outputs in our case an image, therefore it represent the probability distribution $p(x|z; \theta_g)$. The discriminator on the other hand has the objective of understanding if the input comes from the dataset or it's generated. It outputs a scalar value $D(G(z)) \in [0, 1]$.

The loss function for the GAN  text is:


$\begin{align}\underset{G}{\text{min}} \underset{D}{\text{max}}V(D,G) = \mathbb{E}_{x\sim p_{data}(x)}\big[logD(x)\big] + \mathbb{E}_{z\sim p_{z}(z)}\big[log(1-D(G(z)))\big]\end{align}$

In pytorch we can implement this loss via the [BCE loss](https://pytorch.org/docs/stable/generated/torch.nn.BCELoss.html) where the target label is 1 if the image is real of 0 if it's generated. 