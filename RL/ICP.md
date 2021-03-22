## Iterative Closest Point 

For each point the best candidate (for a point in the first cloud) for a corresponding point is the point (in the second cloud) that is closest to it now 

Steps: 
- Initial guess for the transformation, since there is no guarantee for convergence. the intial guess can come from the IMU model, or in general a motion model.
- Associate each point P_{s'} with the nearest point in P_{s}
- Minimizing the sum of square distances find the optimal transformation