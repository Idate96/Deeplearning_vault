The points of the cloud are given w.r.t the pose (coordinate frame) S of the lidar.
![[Screenshot 2020-11-13 at 09.06.10.png]]
At the next time step the point clouds will be given wr.t a new coordinate frame S', since the car has moved in the last time-step. 

## Point set registration problem
We want to find the optimal *rotation* and *translation* that minimizes the distance between the two point clouds (from the same object).

![[Screenshot 2020-11-13 at 09.07.42.png]]

In general we cannot match points exactly between each other. 
The most popular algorithm is called [[ICP]]