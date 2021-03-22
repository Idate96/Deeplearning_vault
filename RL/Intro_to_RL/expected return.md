#definition 
Sum of rewards for a finite episode of lenght T:
$G_t = R_{t + 1} + R_{t + 2} + ... + R_{T}$
Note that the index "goes" into the future

### Cumulative Expected Return 
$G_{t} = R_{t + 1} + \gamma R_{t + 2} + \gamma^2 R_{t + 3} + ... = \sum_{i = 0}^{\infty} \gamma^{i} R_{t + 1 + i}$

$G_t = R_t + \gamma G_{t + 1}$
gamma is called the discount rate.

Discounted returns are necessary to keep the value of the reward bounded in an infinite episode. 

