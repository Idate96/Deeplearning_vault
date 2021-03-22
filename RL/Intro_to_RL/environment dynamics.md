#definition
The environment dynamics define the probability of reaching a next state s' and obtaining reward r given action a and state s:

$p(s',r|a, s) = Pr(S_t = s', R_t = r| S_{t-1} = s, A_{t -1} = a)$

since it's a proability distribution:

$\sum_{s' \in S} \sum_{r \in R} P(s', r| s, a) = 1$ for all s, a. 


