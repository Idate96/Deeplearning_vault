Instead of selecting random plans, we first sample from the distirbution. Then we fit a new distirbution to the space of plan that look more promising where the better actions might lie. Now we sample from this new distribution that seems more promising. 

![[Screenshot 2020-11-03 at 14.45.38.png]]

The limitation of this method is a very harsh limit on dimensionality and you can only do open loop planning. 

[Tutorial](http://web.mit.edu/6.454/www/www_fall_2003/gew/CEtutorial.pdf)