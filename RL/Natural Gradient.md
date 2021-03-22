This is used in [[Trust region policy optimization]]. 

Q:*Can I be invariant under a non-linear reparametrization of the space?*

If you have a parametrized probability distribution: $p(x, \theta)$, what if I parametrized with inverse $\theta$, would I get the same behaviour?

There is hope because there is a measure of distance between distributions that is indipendent of the parameters of the distributions:
$KL(p(x; \theta)||q(x;\Psi)) = \int_x p(x;\theta) log \frac{p(x; \theta)}{q(x;\Psi)}$

Consider the following problem
$\max_{\theta} f(\theta) = \max_{\theta} \sum_i \log p(x^(i); \theta)$
It's gradient
$\frac{\partial f(\theta)}{\partial \theta_p} = \sum_i \frac{\partial p(x^{(i)} \theta)}{\partial \theta_p}  p(x^(i)' \theta)$
and the hessian is 
$\nabla_{\theta}^2 f(\theta) = \sum_i \frac{\partial^2 p(x^{(i)}; \theta)}{\partial^2 \theta} \frac{1}{p(x^{(i)}; \theta)} - (\nabla \log p(x^{(i)}; \theta))(\nabla \log p(x^{(i)}; \theta))$

Let's ingnore (local approximation) the first term of the hessian and consider the second term (a negative sign quadratic)

$H \approx \sum_i (\nabla \log p(x^{(i)}; \theta))(\nabla \log p(x^{(i)}; \theta)^T)$

Now we can apply [[Newton's Method]] to find the minimum to the problem:

$x_{i + 1} = x_{i} + t \frac{1}{H} \nabla f(x)$

Now we definte the *Natural Gradient* to be:

$\nabla_{n} f(x) = (\sum_i (\nabla \log p(x^{(i)}; \theta))(\nabla \log p(x^{(i)}; \theta)^T)^{-1} \sum_i \nabla \log p(x^{(i)}, \theta)$

**This is invariant to general (not only affine) reparametrization**
