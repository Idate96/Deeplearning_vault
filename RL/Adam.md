In adam you have both the velocity and the variance
$v = \beta_1 v + (1 - \beta_1) \frac{\nabla_x f(x)}{(1-\beta^{t}_1)}$
$s = \beta_2 s + (1 - \beta_2)(\nabla_x f(x))^2) / (1 - \beta^{t}_2)$
the update 
$x = x + \alpha \frac{v}{\sqrt{s + \epsilon}}$
