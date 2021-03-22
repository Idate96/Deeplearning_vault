Consider 
$\dot{x} = f(x) + g(x)u$
if $g(x)$ is square (number of control is the same as number of states) and invertible then we can define the following virtual input:
$v(x) = f(x) + g(x)u$
which linearizes the system to $\dot{x} = v$

### Example 
For manipulator we have the following equation:
$H(q)\ddot{q} + b(\dot{q}, q) + g(q) = \tau$
We can feedback linearize by choosing :
$v = H(q)^{-1}(\tau - b(\dot{q}, q) - g(q))$

Q: *Can we do this when the number of control inputs is not the same as the number of states?*
It's possible but often you won't know how to do it.
