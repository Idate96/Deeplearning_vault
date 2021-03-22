We first define a velocity:
$v = \beta v + (1 - \beta) \nabla_x f(x)$
and the update:
$x = x + \alpha v$

If you have a high dimensional problem and you do back and forth a lot, the change in direction will average out (velocity) and point you in the correct direction. 
This is using a bit of second order of information (more than one gradient)
$\beta \approx 0.99$