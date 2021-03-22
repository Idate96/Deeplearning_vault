It uses two networks, each with indipendent noise, with parameter $\phi$ and $\phi'$.  We can use the current and target networks in the following way to set the target label 

$y = r + \gamma Q_{\phi'}(s', \arg \max_{a'} Q_{\phi}(s', a'))$

Instead of 

$y = r + \gamma Q_{\phi'}(s', \arg \max_{a'} Q_{\phi'}(s', a'))$

Use current network to evaluate action and target network to evaluate the value.
The target network is updates only every N steps