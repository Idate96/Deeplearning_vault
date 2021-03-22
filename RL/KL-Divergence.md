The divergence between two probability distributions $q(x)$ and $p(x)$

$D_{KL}(q||p) = E_{x \sim q(x)} [\log \frac{q(x)}{p(x)}] = E_{x \sim q(x)} [\log q(x)] - E_{x \sim q(x)} [\log p(x)] = - E_{x \sim q(x)} [\log p(x)] - H(q(x))$

Intuition:
1. How different are two distributions?
2. how small is the expected log probability of one distribtuion under another, minus the entropy ? When minimizing the KL divergence, the first part is maximized by sampling z close to the peak of p(x), so q(x) will be almost like a delta function. The entropy will spread out more the distribution. 

