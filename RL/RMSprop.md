It solves the problem of going back and forth, in the optimization landscape, by looking at the variance of the gradients. If the gradient oscillates a lot, you should not trust it much. 
the variance is:
$s = \beta s + (1 - \beta)(\nabla_x f(x))^2$
and the update:
$x = x + \alpha \frac{\nabla_x f(x)}{\sqrt{s + \epsilon}}$
$\epsilon \approx 10^{-8}$ to avoid numerical problems