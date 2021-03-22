Since 
$P(x, y) = P(x|y) P(y) = P(y|x) P(x)$
Then 
$P(x|y) = \frac{P(y|x)P(x)}{P(y)}$
where 
$P(y) = \sum_x P(y|x)P(x)$

### With conditioning 
$P(x|y, z) = \frac{P(y|x, z)P(x|z)}{P(y|z)}$

### Recursive Bayesian Updating 
What if we want to combine evidence, $z_1, z_2, ...$, how do we estimate $P(x|z_1, ..., z_n)$?

$$
P(a|z_1, ..., z_n) = \frac{P(z_n|x, z_1, ..., z_{n-1})P(x|z_1,..., z_{n-1})}{P(z_n|z_1,...,z_{n-1})}
$$

We assume the markov property: $z_n$ is indipendent of $z_1, ..., z_{n-1}$, if we know x: $P(z_n|x, z_1, ..., z_{n-1}) = P(z_n|x)$

This implies 
$$
\begin{align}
P(x|z_1, ..., z_n) = \frac{ P(z_n|x)P(x|z_1,..., z_{n-1})}{P(z_n|z_1,...,z_{n-1})} \\
= \eta P(z_n|x)P(x|z_1, ..., z_{n-1})  \\
= \eta_{1, ..., n} \left( \prod_{i = 1}^n P(z_i|x) \right) P(x)
\end{align}
$$

*Pitfal*: Real world measurment (or sensor) are not really indipendent (say two beans of LIDAR that encounter the same noise). For example choose the frequency of reading from a sensor (which is still a physical system), such as a gyroscope, to obtain indipendent readings to avoid becoming overconfident in a state. 







