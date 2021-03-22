#cs287 
#lecture 
Remember before we saw tile coding as a way to discretize the MDP.

(Page 6)
Steps to discretize the MDP. Sometime you don't need to discretize actions (thanks to the max). The tricky part is really the transition function. 
We will focus on discretizing it. 

Q:*What happens when we discretize*?
The dynamics model will predict an end state that probably will not match with the discretization of the state space. So you have to project the end state onto the grid (like [[Nearest neighbour]]). But when you stay close to your original place you might be placed back at the beginning. In general there are a lot of problems with this discretization. 

A more natural approach would be to assign a probability over the discrete states after an action is taken. 

(Page 11)
We can also definte the final state as a convex combination of the neighbours 
![[Screenshot 2020-10-30 at 09.02.58.png]]

(Page 17)
Where do you sample the policy?

(Page 21)
## Cross Entropy Method 
It is a way to maximise any function. 

(page 22)
Have a mean as a starting point. 
Improve the mean in the direction, where the best x is. 
You sample x, with the current mean, and evaluate the function value. 
The new mean is the average of the top 10% of the x. 