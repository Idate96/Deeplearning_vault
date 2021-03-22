# The Free Energy principle
**Type**: #essay 

---

## Intro and motivation
How is it that living things tend to select actions that secure their longevity? It is perhaps trivial that otherwise they would not be living, but far from trivial is the stable process that results from striving to stay alive. From a physics perspective, life is a phenomenon stably supported on a minima of a convex energy landscape. The Free Energy principle is one of the most compelling proposals for such a landscape because it can explain the emergence of life and complex intelligent behavior in all the breadth of levels from molecular, to cellular, to neural dynamics, to complex societal organization [`Hesp2019`]. This work takes a perspective from statistical physics to provide a formal definition of Free Energy as it arises in Bayesian model inference. Then, its application in active inference for robotics is briefly introduced.

---
## Origins in variational Bayesian inference
The proposition of Bayesian inference is a framework based on Bayes' theorem for inferring the probability of an hypothesis based on the available evidence, in this case the observations $y$, and the assumption of the model $m$ that generates those observations. A range of well-known schemes are derived within this framework. For instance, for standard linear state-space models with known model parameters and Gaussian noise, the Bayesian inference of hidden states $x$ is generally called Kalman filtering, and if dealing with static models (exempt of hidden states other than causes) the resulting method is principal component analysis [`Friston2008`]. Non-Bayesian approaches to model inference typically fall in the category of sampling methods, such as Monte Carlo methods. The topic of interest in this section is the concept of free-energy which is encountered in the general derivation of Bayesian inference schemes, and in particular in the case of models with Gaussian variables such as [[Hierarchical dynamic models]].

Any model $m$ consists of a set of variables and parameters $\vartheta$. For instance, in a dynamical model these could be hidden states $x$, causes $v$, parameters $\theta$ of the mapping functions ($g(x, v)$ and $f(x, v)$) and hyperparameters $\lambda$ of the noises, such that $\vartheta=\{x, v, \theta, \lambda\}$. On the basis of a choice of model and the available observations, the typical task of a Bayesian inference scheme is to discover the conditional or posterior density $p(\vartheta\mid y, m)$, known as ensemble density, because its mean $\mu^{\vartheta}$ reveals the hypothesis that is the most probable explanation of the observations. The statement of the Bayes' theorem reads
`eq:bayes_inference_formula`
$$
p(\vartheta\mid y, m) = \frac{p(y\mid\vartheta, m)p(\vartheta\mid m)}{p(y\mid m)} \qquad\text{with}\qquad p(y\mid m)=\int_{\Theta} p(\vartheta,y\mid m)d\vartheta
$$
The denominator entails an integral[^1] over the state and parameter space $\Theta$, which is usually too large to be computationally tractable. Enter variational Bayes. Instead of directly inferring the true posterior $p(\vartheta\mid y)$ ($m$ omitted for brevity), the proposal of variational Bayes is to approximate it with a *variational distribution* $q(\vartheta)$. The key advantage is that the family of $q(\vartheta)$ may be chosen; later in this discussion $q$ is assumed Gaussian because of its minimal set of sufficient statistics (e.g. mean and variance). Note how the true posterior, with infinite degrees of freedom, is then synthesized into two values, with some loss of information related to the adequacy of the Laplace assumption on the underlying generative process $p(\vartheta, y)$. The accuracy of the fit of $q(\vartheta)$ is measured in terms of a dissimilarity function, namely the Kullback–Leibler divergence (KL-divergence). Although it cannot be computed explicitly (because the conditional density $p\left(\vartheta\mid y\right)$ is not available) it has two important properties for this discussion: (1) it is strictly positive and (2) it is minimum when $q(\vartheta)\approx p\left(\vartheta\mid y,m\right)$. Expanding the KL formula yields expression
`eq:variational_bayes_derivation`
$$
\begin{aligned}
        D_{\text{\tiny KL}}\left(q(\vartheta)\mid\mid p(\vartheta\mid \tilde{y})\right) &= \int q(\vartheta)\ln\frac{q(\vartheta)}{p(\vartheta\mid \tilde{y})} \text{d}\vartheta \\
		&= \int q(\vartheta)\ln\frac{q(\vartheta)p(\tilde{y})}{p(\vartheta, \tilde{y})} \text{d}\vartheta\\
        &= \int q(\vartheta)\ln q(\vartheta) \text{d}\vartheta - \int q(\vartheta)\ln p(\vartheta,\tilde{y}) \text{d}\vartheta + \ln p(\tilde{y})\int q(\vartheta) \text{d}\vartheta \\
		&= \int q(\vartheta)\ln q(\vartheta) \text{d}\vartheta - \int q(\vartheta)\ln p(\vartheta,\tilde{y}) \text{d}\vartheta + \ln p(\tilde{y})
    \end{aligned}
$$
The first term is the Shannon entropy of $q(\vartheta)$ and the second term is the *internal energy*: how much information could be potentially extracted by $q(\vartheta)$ in a particular occurrence of $y$. Together they define the *free energy* of the system, which borrows its name from thermodynamics; the free energy is total internal energy minus that which cannot be extracted in any useful form, the entropy. In information theory, *energy* is the information content in a particular outcome of a random variable, also known as negative log-likelihood because of its expression $I(\vartheta)=-ln(p(\vartheta))$ (see `fig:surprisal`[^2] for an example). The last term is the (negative) *surprise* of the observation, which quantifies how much of its information content remains unexplained by the model. A small rearrangement results in the expression
`eq:variational_bayes`
$$
-\ln p(\tilde{y}) \approx \int q(\vartheta)\ln q(\vartheta) \text{d}\vartheta - \int q(\vartheta)\ln p(\vartheta,\tilde{y}) \text{d}\vartheta - D_{\text{\tiny KL}}\left(q(\vartheta)\mid\mid p(\vartheta\mid \tilde{y})\right)
$$
which, after having found a fitting $q(\vartheta)$ that renders the $D_{\text{\tiny KL}}\approx 0$, naturally reads: *the surprisal of observation $y$ is equal to the instantaneous free energy of the model*. In other words, the Free Energy is an upper bound to surprise (because $D_{\text{\tiny KL}}$ is strictly positive). The implications of this finding form the basis of the Free Energy principle: that any system upholding its own integrity must be minimizing its free energy. For an intelligent system it means maintaining an internal model of the world, both through perception as well as action [`Friston2006`] ([[Perception and action]]).

[^1]: This integral receives the name of *marginal* over random variable $\vartheta$ because it excludes it from the joint probability.
[^2]: `fig:surprisal` Surprisal as a function of the probability of an observation. An observation that is not expected produces high surprisal energy. Note that probability densities may assume values higher than 1, in which case their energy becomes negative. This nonetheless works perfectly well with the mathematical equations in variational Bayesian inference. ![[surprisal.png|400]]

---
## Closed forms of the Free Energy
More than a theoretical curiosity, under the Laplace assumption `eq:variational_bayes` can be derived into a closed-form expression that completely eliminates the intractable integrals. This gives rise to powerful biologically-plausible inference schemes that revolve around the idea of predictive coding [`Friston2003a`], and which may be of great interest for robotics. The objective of this section is to derive the equality
`eq:free_energy_closed_form`
$$
\begin{aligned}
F 
&= \int q(x)\ln q(x) \text{d}x - \int q(x)\ln p(y,x) \text{d}x \\
&= \langle\ln q(x)\rangle_q - \langle \ln p(y,x)\rangle_q
\end{aligned}
$$
where $\vartheta$ has been replaced by $x$ because it makes the intermediate expressions easily recognizable as standard statistical formulas and where $\langle*\rangle_q$ indicates the expected value under $q$ (i.e. $\langle\ln q(x)\rangle_q=\ln q(\mu)$, $\mu$ being the mean value of $q$). First, the Gaussian form $q(x)=N(x:\mu,\sigma)$ is substituted in the first integral of `eq:free_energy_closed_form`
`eq:derivation_first_integral_free_energy`
$$
\begin{aligned}
        \int q(x)\ln q(x) \text{d}x &= \int q(x)\ln\left( \frac{1}{\sqrt{2\pi \sigma^2}}\;e^{-\frac{1}{2}\frac{(x-\mu)^2}{\sigma^2}}\right) \text{d}x \\
        &= \int q(x)\left(\ln\left( \frac{1}{\sqrt{2\pi \sigma^2}}\right)-\frac{1}{2}\frac{(x-\mu)^2}{\sigma^2}\right) \text{d}x \\
        &= \ln\left(\frac{1}{\sqrt{2\pi \sigma^2}}\right)\underbrace{\int q(x) \text{d}x}_{=1}-\frac{1}{2\sigma^2}\underbrace{\int q(x)(x-\mu)^2\text{d}x}_{=\sigma^2} \\
        &= \ln\left(\frac{1}{\sqrt{2\pi \sigma^2}}\right) - \frac{1}{2} \\
        &= \ln q(\mu) - \frac{1}{2} \equiv ln q(\langle x\rangle_q )- \frac{1}{2}
    \end{aligned}
$$
Although a univariate distribution has been used in this derivation, it is easy to see that the same applies to multivariate distributions $N(\boldsymbol{x}:\boldsymbol{\mu},\Sigma)=((2\pi)^n|\Sigma|)^{-1/2} \exp((\boldsymbol{x}-\boldsymbol{\mu})\Sigma^{-1}(\boldsymbol{x}-\boldsymbol{\mu}))$ with $\boldsymbol{x}\in\mathbb{R}^n$. The rest of the derivation continues on the univariate case.

The second term in `eq:free_energy_closed_form` contains the unknown distribution $p(y,x)$. Nonetheless, we can start working towards a solution by approximating the internal energy $U(y,x) = \ln p(y,x)$ with a Taylor expansion around $x=\mu$ (the mean of $q$)
`eq:derivation_second_integral_free_energy`
$$
\begin{aligned}
        \int q(x)U(y,x) \text{d}x
        &= \int q(x)\left(U(y,\mu) + \underbrace{U_x(y,\mu)}_{=0}(x-\mu)+\frac{1}{2}U_{xx}(y,\mu)(x-\mu)^2\right)\text{d}x \\
        &= U(y,\mu)\underbrace{\int q(x)\text{d}x}_{=1} + \frac{1}{2}U_{xx}(y,\mu)\underbrace{\int q(x)(x-\mu)^2\text{d}x}_{=\sigma_q^2} \\
        &= U(y,\mu) + \frac{1}{2}U_{xx}(y,\mu)\sigma_q^2 = \langle U(y,x)\rangle_q + \frac{1}{2}U_{xx}(y,\mu)\sigma_q^2\\
    \end{aligned}
$$
The first derivative $U_x(y,\mu)$ is zero because the internal energy is maximal at $x=\mu$. The expression for $U_{xx}(y,\mu)$ requires some discussion. First, an alternative expression for $U(y,x)$ is
$$
\begin{aligned}
U(y,x)&=-\ln(p(y,x))\\
&=-\ln(p(y\mid x)p(x))\\
&=-\ln(p(y\mid x))-\ln(p(x))
\end{aligned}
$$
The first term describes the energy of a predictive model that delivers the surprise in an observation, the second is the entropy in the latent variables that generated the expectation $g(x)$. Again, taking the Laplace assumption such that $p(y\mid x)=N(y:g(x),\sigma_z)$ and $p(x)=N(x:\mu,\sigma_p)$,
$$
U(y,x)=-\frac{(y-g(x))^2}{2\sigma_z^2}-\frac{(x-\mu)^2}{2\sigma_p^2}
$$
and the second-order term is then 
$$
\begin{aligned}
U_{xx} &= -\underbrace{\frac{\partial^2}{\partial x^2}\frac{(y-g(x))^2}{2\sigma_z^2}}_{=0}-\frac{\partial^2}{\partial x^2}\frac{(x-\mu)^2}{2\sigma_p^2} \\
&= \frac{1}{\sigma_p^2}
\end{aligned}
$$
where higher order terms like $g_{xx}(x)$ and $g_x^2(x)$ have been ignored under the assumption that the generative model is only weakly nonlinear near the mode [`Friston2006a`]. Substituting this result into `eq:derivation_second_integral_free_energy` and adding `eq:derivation_first_integral_free_energy` results in an expression close to `eq:free_energy_closed_form`
$$
F=\langle\ln q(x)\rangle_q - \langle \ln p(y,x)\rangle_q + \frac{1}{2}\left(\frac{\sigma_q^2}{\sigma_p^2}-1\right)
$$
Since $q$ is a surrogate model of $p$ it follows that $\sigma_q^2\geq \sigma_p^2$ and therefore the last term is bounded in $[-\tfrac{1}{2}, 0]$. Because of this, the first two terms still define an upper bound for surprise and therefore they suffice for optimization. Another argument is that as the divergence  $D_{\text{\tiny KL}}$ is minimized the variance $\sigma_q$ must come closer to the true variance $\sigma_p$, so this last term converges to zero as the optimization progresses and hence it is safe to ignore.

In conclusion, variational Bayesian inference converts a hard computation problem into a convex optimization under the Laplace assumption. Despite the apparent limitations of the Gaussian forms, as well as the assumption of only mild nonlinearities in the generative density $g(x)$, it has been argued through empirical results that the brain may be exploiting these very same assumptions, and adding model flexibility instead through hierarchical compositions [`Friston2008`, ==missing more references==]. For robotics applications, the closed-form is often used in a more explicit form called the Laplace-encoded Free Energy, discussed next.

## Laplace-encoded Free Energy
==to do==

$\langle log(x)\rangle_q = log(\mu) - \frac{\sigma^2}{2 \mu^2} =  log(\langle x \rangle_q) - \frac{\sigma^2}{2 \mu^2}$

$\langle log(x)\rangle_q \neq log \langle x\rangle_q = log(\mu)$