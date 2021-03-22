# Hierarchical Dynamic Models
**Type**: #essay

---
## Intro and motivation
An observed motion can generally be traced back to root forcing functions through a causal chain. The convolution applied by this chain on the original triggers is often referred to as the generative process. The length and transformation functions of these chains can be arbitrarily defined, with each causal link aggregating circumstantial effects to incoming dynamics and adding additional dynamics to the observed signals. Each stage may also be subjected to process noise, which blurs the components of the signal and further complicates solving the reverse traversal from observation to causes (known as the blind deconvolution problem). This section presents an expansion of standard state-space models to probabilistic generative models[^1] in generalized coordinates and introduces a hierarchical framework with these generative models embedded as nodes. This generally applicable model finds its use in Bayesian forecasting and its inversion (discussed in [[Variational bayesian inference]]) yields inference schemes that closely resemble empirical results in human perception [`Friston2008`].

[^1]: A *generative model* is a statistical model of the joint probability distribution $P(X, Y)$ of an observable variable $Y$ and a latent variable $X$.

---
## State-space in generalized coordinates
The standard state-space model describes the dynamics of a state due to its own inertia $\dot{x}$ and additional causes $v$ (also referred to as inputs or sources) in configuration space, and outputs the signal $y$ in observable space. The mapping $f$ from states and causes to state inertia in configuration space are the equations of motion, and the mapping $g$ from configuration to observable space is the response function. Uncertainties in both mappings are modeled by Gaussian process noises $z$ and $w$. This has the familiar expression
`eq:standard_state_space`
$$
y=g(x,v) + z  \qquad\qquad
\dot{x}= f(x,v) + w
$$

Of course, the elements of a state-space model may be vectors and the mapping functions may be non-linear. The flexibility this confers to the models has made them a common choice in virtually any discipline. One pitfall that is often overlooked and sometimes with frustrating consequences is that this model is a formal statement of a standard Markov processes. The Markov assumption ignores higher orders of motion and dumps them into the process noises, which results in infinitely rough motion (here the mathematical definition of roughness is meant, related to the truncation of a Taylor expansion). This is unrealistic for any signal originating from a real environment and will fail to capture fluctuations at short time scales [`Stratonovich1964`, `Friston2008`]. The solution is simply to recursively differentiate the standard form so as to express it in generalized coordinates $\tilde{x} = [x, x', x'', ...]$, and similarly for the other variables $\tilde{v}, \tilde{y}, \tilde{z}, \tilde{w}$. For simplicity, the mapping functions are assumed locally linear and their higher derivatives are ignored. The equation below shows the state-space equations extended to generalized coordinates.
`eq:generalized_state_space`
$$
\begin{aligned}
	y&=g(x,v) + z                                       \qquad& \dot{x}&=x'= f(x,v) + w \\
	\color{gray}y'&\color{gray}=g_x x'+g_v v' + z'      \qquad& \color{gray}\dot{x}'&\color{gray}=x''= f_x x'+f_v v' + w' \\
	\color{gray}y''&\color{gray}=g_x x''+g_v v'' + z''  \qquad& \color{gray}\dot{x}''&\color{gray}=x'''= f_x x''+f_v v'' + w'' \\
	&\qquad\vdots & &\qquad\vdots
\end{aligned}
$$
This can be alternatively expressed compactly in vector form using the block-matrix derivative operator $D$ (see [[Friston conventions and notations]])
`eq:generalized_state_space_compact`
$$
\tilde{y} = \tilde{g} + \tilde{z} \qquad\qquad
D\tilde{x} = \tilde{f} + \tilde{w}
$$

---
## Probabilistic dynamic models
In the previous section, and in the standard state-space model, the variables are assumed real-valued (i.e. from sample space). Nonetheless, in order to define generative models the variables must be considered as probability density functions (i.e. from probability space). If the process noises are assumed Gaussian such that $p(\tilde{z})=N(\tilde{z}:0,\tilde{\Sigma}^z)$ and $p(\tilde{w})=N(\tilde{w}:0,\tilde{\Sigma}^w)$, the likelihood $p(\tilde{y}\mid\tilde{x},\tilde{v})$ and the empirical priors $p(\tilde{x}\mid\tilde{v})$ have too Gaussian densities
`eq:gaussian_densities`
$$
\begin{aligned}  		p(\tilde{y}\mid\tilde{x},\tilde{v})&=N(\tilde{y}:\tilde{g},\tilde{\Sigma}^z) \\
p(\tilde{x}\mid\tilde{v})&=N(D\tilde{x}:\tilde{f},\tilde{\Sigma}^w)
\end{aligned}
$$
The density on the hidden states $p(\tilde{x}\mid\tilde{v})$ incorporates the constraint
`eq:generalized_coord_constraint`
$$
p(\tilde{x})=p(D\tilde{x})
$$
which defines a model of correlated noise, and implies that as long as the high-order motion holds some precision, the Markov assumption is invalid because it violates the constraint by dumping these orders and hence introduces random errors that propagate to the low-order motion (fast dynamics). Later it is shown that the embedding order of the generalized coordinates need not be very high in practice because the precision of the high-order terms tends to decay exponentially. Assuming that the generalized causes are also Gaussian priors $p(\tilde{v})=N(\tilde{v}:\tilde{\eta},\tilde{\Sigma}^v)$ completes the generative model
`eq:generative_model_single_layer`
$$
\begin{aligned}	p(\tilde{y},\tilde{x},\tilde{v})&=p(\tilde{y}\mid\tilde{x},\tilde{v})p(\tilde{x},\tilde{v}) \\
p(\tilde{x},\tilde{v})&=p(\tilde{x}\mid\tilde{v})p(\tilde{v})
\end{aligned}
$$
Despite the apparent abuse of the Gaussian assumption in a model that claims to stay generally applicable, the assumption is not restrictive on the type of distribution of the observations because the terms $\tilde{g}$ $\tilde{f}$, $\tilde{\Sigma}^z$ and $\tilde{\Sigma}^w$ can embody probability integral transforms that reparameterize non-Gaussian processes to Gaussian latent variables [`Friston2008a`, `Ziegel1997`]. Still, the choice for Gaussian distributions may sound rather arbitrary, other than because they make the treatment easier. Perhaps a more compelling reason is that, having drawn inspiration from the human brain for his derivation, Friston argues that synapses and related neural processes are not known to represent anything but unimodal latent states [`Friston2008`], in which case Gaussian forms are a trivial choice, attending for their ubiquity in natural processes.

Returning to the ==constraint on hidden state dynamics==, it defines a correlation between fluctuations at different orders (e.g. an integrated fluctuation in $x'$ has the same variance as process fluctuations in $x$, and conversely, the differentiated noise at $x$ has the same distribution as noise in $x'$). Since the different orders of the noise "talk to each other", the autocorrelation $\rho(0)$ of the signal will have finite derivatives, which themselves describe the relative variance that those orders hold on the zero-order. In a Markov process the noise levels are uncorrelated (have infinite relative variance or zero relative precision) and the autocorrelation is a discontinuous unit-peak at zero (plus smaller peaks from spurious correlations). The derivatives can be arranged in a generalized relative variance (GRV) matrix, the inverse of which is the so-called temporal precision matrix $S(\gamma)$. The parameter $\gamma$ controls the strength of the constraint and hence the extent of the autocorrelation. It's a design parameter of the model rather than a property of the process being modeled, but it must be tuned to the capture the most of fluctuations and ignore the unimportant. To understand the construction of the GRV matrix it is key to keep in mind the properties of an autocorrelation function: (1) it is an even function and (2) peaks at $\rho(0)$ with value 1. The first property implies that every odd derivative will be zero, and the second property adds that even derivatives will alternate sign at $\rho(0)$.
`eq:temporal_precision_matrix`
$$
S(\boldsymbol{\gamma})^{-1}=
    \begin{bmatrix}
        1 & 0 & \rho^{[2]}(0) & 0 & \rho^{[4]}(0) & \dots \\
        0 & -\rho^{[2]}(0) & 0 & -\rho^{[4]}(0) & 0 \\
        \rho^{[2]}(0) & 0  & \rho^{[4]}(0) & 0 & \rho^{[6]}(0)\\
        0 & -\rho^{[4]}(0) & 0 & -\rho^{[6]}(0) & 0  \\
        \rho^{[4]}(0) & 0 & \rho^{[6]}(0) & 0 & \rho^{[8]}(0) \\
        \vdots & & & & & \ddots
    \end{bmatrix}
$$
The properties of the autocorrelation function lead again to the choice of a Gaussian form for $\rho$. Friston chooses $\rho=e^{-\tfrac{x^2\gamma}{4}}$ which leads to the closed-form expression
`eq:temporal_precision_matrix_gaussian`
$$
    S(\boldsymbol{\gamma})^{-1}=
    \begin{bmatrix}
        1 & 0 & -\tfrac{1}{2}\gamma & 0 & \tfrac{3}{4}\gamma^2 & \dots \\
        0 & \tfrac{1}{2}\gamma & 0 & -\tfrac{3}{4}\gamma^2 & 0 \\
        -\tfrac{1}{2}\gamma & 0  & \tfrac{3}{4}\gamma^2 & 0 & -\tfrac{15}{8}\gamma^3\\
        0 & -\tfrac{3}{4}\gamma^2 & 0 & \tfrac{15}{8}\gamma^3 & 0  \\
        \tfrac{3}{4}\gamma^2 & 0 & -\tfrac{15}{8}\gamma^3 & 0 & \tfrac{105}{16}\gamma^4 \\
        \vdots & & & & & \ddots
    \end{bmatrix}
$$
The rule for finding the coefficients without explicitly differentiating $\rho$ has been derived by another student [`Hijne2020`] and is listed in [[Friston conventions and notations]]. One notable feature of $S(\boldsymbol{\gamma})^{-1}$ is that for $\gamma>1$ the GRV of higher-order terms increases exponentially. The inverse $S(\boldsymbol{\gamma})$ is graphically shown in the image below for the choice of $\gamma=4$ (black tiles represent zero values, elsewhere the values are positive). The temporal precision clearly plummets after the 6th order. Friston uses this argument to define the sufficient embedding order of the generalized coordinates.
![[temporal_precision_matrix.png| 300]]
The sampling variances $\tilde{\Sigma}^z$ and $\tilde{\Sigma}^w$and their precisions are simply
`eq:generalised_variance_precission`
$$
\begin{aligned}
\tilde{\Sigma}^z &= S(\boldsymbol{\gamma})^{-1}(\sigma^2)^z & &\Leftrightarrow & \tilde{\Pi}^z &= S(\boldsymbol{\gamma})\frac{1}{(\sigma^2)^z} \\
\tilde{\Sigma}^w &= S(\boldsymbol{\gamma})^{-1}(\sigma^2)^w & &\Leftrightarrow & \tilde{\Pi}^w &= S(\boldsymbol{\gamma})\frac{1}{(\sigma^2)^w}
\end{aligned}
$$

This concludes a formal definition of probabilistic dynamic models in generalized coordinates. Before continuing onto hierarchical structures, a small addition on the use of generalized coordinates when these are not available from the data. It is often the case that a model is tasked with the recognition of time series data with discrete streams of scalar observations rather than continuous functions. A simple conversion from discrete data with sample time $h$ to generalized coordinates is mediated by the Taylor expansion [`Friston2008a`]
`eq:discrete_sequence_to_generalised`
$$
y_{k+i} = \sum_{j=0}^p\frac{(ih)^j}{j!}y_k^{[j]}  \qquad\Leftrightarrow\qquad \boldsymbol{y}=\tilde{E}\,\tilde{y} \quad,\quad \tilde{E}_{ij}=\frac{(ih)^j}{j!} \quad\Rightarrow\quad \tilde{y}=\tilde{E}^{-1}\boldsymbol{y}
$$
in which the length of the sample $\boldsymbol{y}$ must be consistent with the embedding order of the generalized coordinates ($N = p+1$) so that $\tilde{E}$ is invertible. Forward or backward derivatives are possible by using positive or negative $i$ (and ignoring the symbol when using it as matrix index). The accuracy of these differentiation schemes is analyzed in [`Hijne2020`], and it is made clear that with increasing embedding order the accuracy of the computed generalized coordinates rapidly becomes problematic. Friston notes that this is not an issue in systems that deal with continuous streams of data, such as the brain [`Friston2008`].


## Adding hierarchy
The state-space models from the previous section model information flow from causes to observations through hidden states. The variables may be multidimensional vectors and the mapping functions may be arbitrary such that complex multivariate interactions can be modeled. In other words, it enables the construction of any Bayesian dependency graph. However, motion in the real world has a certain structure of conditional independencies defined by causal chains. For instance, a bicycle moves because we pedal and not the other way around; the pedaling is conditionally independent of the bicycle movement and instead depends on our intention (which certainly takes feedback from the motion, but that control loop is not part of the dynamical model of the bicycle). Of course, functions $g$ and $f$ could be engineered to represent this dynamic causal chain, but an alternative and simpler approach is to chain simpler dynamical models. This is nothing more than a special case of `eq:generalized_state_space_compact` where a hierarchical structure systematically removes meaningless edges from the Bayesian dependency graph (the result is a so-called Markov blanket). Note that the notation for a variable $a$ in layer $i$ is $a^{(i)}$, whereas for coordinates within a generalized variable the notation $a^{[i]}$ is used instead. By using the shorthand expressions $\tilde{g}^{(i)}=\tilde{g}(\tilde{x}^{(i)}, \tilde{v}^{(i)})$, and similarly for $\tilde{f}^{(i)}$, the hierarchical expansion of the generalized state-space model becomes
`eq:generalized_state_space_hierarchical`
$$
\begin{aligned}
\tilde{y} &= \tilde{g}^{(1)} + \tilde{z}^{(1)} & \qquad&\qquad & D\tilde{x}^{(1)} &= \tilde{f}^{(1)} + \tilde{w}^{(1)} \\
\tilde{v}^{(1)} &= \tilde{g}^{(2)} + \tilde{z}^{(2)} & \qquad&\qquad & D\tilde{x}^{(2)} &= \tilde{f}^{(2)} + \tilde{w}^{(2)} \\
&\qquad\vdots & \qquad&\qquad & &\qquad\vdots \\
\tilde{v}^{(i-1)} &= \tilde{g}^{(i)} + \tilde{z}^{(i)} & \qquad&\qquad & D\tilde{x}^{(i)} &= \tilde{f}^{(i)} + \tilde{w}^{(i)} \\
&\qquad\vdots & \qquad&\qquad & &\qquad\vdots \\
\tilde{v}^{(m)}&=\tilde{\eta}+\tilde{z}^{(m+1)} & & & & \\
\end{aligned}
$$
Causes $\tilde{v}^{(i)}$ are outputs from the intermediate levels that are fed into the next level, the last being the observations $\tilde{v}^{(0)}=\tilde{y}$. The top level cause is externally provided (for instance, when used as a forward model in a model-based controller). Note that this model is non-Markov in the generalized coordinates but it does have a Markov property over the hierarchy's levels. In the biking example, the motion is solely dependent on the pedaling, regardless if that comes from our own muscles or from a motor; the observation *motion* is conditionally isolated from the cause *muscle activation*, but the cause *pedaling* can mediate the dynamics between our muscle action and the bicycle motion. The causes $[\tilde{v}^{(1)},\dots,\tilde{v}^{(m)}]$ link levels and the hidden states $[\tilde{x}^{(1)},\dots,\tilde{x}^{(m)}]$ link dynamics over time [`Friston2008`]. An earlier account to this method are parametric empirical Bayes models, with only two-layer hierarchies and where the causes are seen as static priors for the level below [`Kass1989`]. In `eq:generalized_state_space_hierarchical`, the hierarchy may have many levels and the causes are dynamic (due to the use of generalized coordinates).

The hierarchical form can also be translated to a generative model by using a the same Gaussian forms as in `eq:gaussian_densities`
`eq:gaussian_densities_hierarchical`
$$
\begin{aligned}
p(\tilde{v}^{(i-1)}\mid\tilde{x}^{(i)},\tilde{v}^{(i)})&=N(\tilde{v}^{(i-1)}: \tilde{g}^{(i)}, \tilde{\Sigma}^z)\\
    p(\tilde{x}^{(i)}\mid\tilde{v}^{(i)})&=N(D\tilde{x}^{(i)}: \tilde{f}^{(i)}, \tilde{\Sigma}^w)
\end{aligned}
$$
The addition of the top-level prior $p(\tilde{v}^{(m)})=N(\tilde{v}^{(m)}:\tilde{\eta},\tilde{\Sigma}^v)$ completes the requisites for the generative model
$$
p(\tilde{y}, \tilde{x}^{(1)}, \dots, \tilde{x}^{(m)}, \tilde{v}^{(1)}, \dots, \tilde{v}^{(m)})=p(\tilde{v}^{(m)})\prod_{i=1}^{m-1}p(\tilde{x}^{(i)}\mid\tilde{v}^{(i)})p(\tilde{v}^{(i-1)}\mid\tilde{x}^{(i)},\tilde{v}^{(i)})
$$
The following diagram compares the dynamical model and hierarchical dynamical model. Next to the variable flow diagrams are the contributions to the dynamics (in black) and the contributions to the generative model (in light blue).
![[dynamical_hierarchical_comparison.png]]