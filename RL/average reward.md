A formulation for continouing tasks. 

For example: 
![[Screenshot 2020-10-22 at 14.37.47.png]]

$r(\pi) = \frac{1}{h} \sum_h E_{\pi}[R_t|S_0, A_{0:t-1} \~ \pi ]$

Or

$r(\pi) = \sum_s \mu_{\pi}(s) \sum \pi(a|s) \sum_{s', r} p(r, s'|a, s) r$

We need to introduce the differential return

$G_{t} = R_{t + 1} - r(\pi) + R_{t + 2} - r(\pi) + ...$
