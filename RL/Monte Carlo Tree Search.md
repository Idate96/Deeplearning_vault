Particularly common in board games!

If you have a dicrete set of actions you can evaluate the consequences of choosing or not the first action. Then you can expand the tree to the second action. Unfortunately this is an exponential problem!

![[Screenshot 2020-11-03 at 15.02.46.png]]

Q:*How do we approximate the value a state without the full tree?*
You can cap the tree depth and run a baseline policy to be applied when you reach the leaf, to estimate the value of the action. 

![[Screenshot 2020-11-03 at 15.04.17.png]]

Q:*Which subtrees we search first?*
Let' say we pick $a_1 = 0$ and we get a cumulative reward $r = r(a_1, s_1) + \sum_{t = 2}^N r(s_i, a_i)$ where the actions are rolled out from the baseline (often random) policy. 
Now we can take the other action and we get +15.
![[Screenshot 2020-11-03 at 15.06.59.png]]

We are planning in a stochastic setting (because our policy is random and the consequence of choosing action $a_1$ is not deterministic), so the reward values should be treated as *sample estimates*. 

The heuristic is to choose nodes with best rewards and favor unexplored nodes. 

### Algorithm
1. With the current tree,  using a strategy on how to expand the tree select a leaf (TreePolicy)
2. expand the leaf using your default policy (like the random policy). You can expand the tree first reaching the leaf with the current policy and then using the baseline 
3. change the values of the estimates betwen the current state and the leaf state 

At the beginning our policy can't do anythign very smart. 

At the first iteration:
![[Screenshot 2020-11-03 at 15.12.42.png]]

A common policy for the tree is the 
UCT TreePolicy($s_t$):
- if the state is not fully expand it choose a new action
- else pick the leaf with the best score 

Choose a node based on the average value plus a bonus for rarely visited nodes. 
![[Screenshot 2020-11-03 at 15.20.52.png]]

This policy is recursive!

The next time iteration ($i = 2$) (the indeces of the state indicate the iteration number):
- step 1: expand $s_2$
- step 2: find that value of $s_3$ is 10
![[Screenshot 2020-11-03 at 15.15.24.png]]
- step 3: update previous values
![[Screenshot 2020-11-03 at 15.16.50.png]]



Once the iterations are done we can select the next action.
![[Screenshot 2020-11-03 at 15.38.31.png]]