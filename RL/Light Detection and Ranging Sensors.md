#lidar

It provides 3D scans of the environment. 

Popular modules:
![[Screenshot 2020-11-13 at 09.28.56.png]]

## History
First introduced in the 1960, first used to measure water clouds. Now it's commonly used to map the surfaces of the Earth and it was used by apollo 15 to scan the surface of the moon. 

## How it works
The laser (why lasers?), send light pulse in the environment. The light pulse is gonna be reflected (a fraction of it) back into the sensor and using the round trip time we can find the distance of the object. 
The intensity of the received pulse (in the photodetector) also can give us information about the type of material of the object. 

![[Screenshot 2020-11-13 at 09.34.27.png]]

The intensity data can be used to generate 2D images, on which classical computer vision algorithm can be applied. This allow the car to "see" in the dark.

Q: *How do we use this to measure different direction?*
Make the lidar rotate around the yaw axis. This gives you a 2D slice. 
The 3D scan can be achieved by adding a nodding motion to the lidar. Alternatively the mirror (in the picture) can be angled differently (as in velodyne lidar)

![[Screenshot 2020-11-13 at 09.38.26.png]]

The round circles are a 2D scan produced by the lidar sensor
![[Screenshot 2020-11-13 at 09.38.58.png]]

This set of points is called a **point cloud**. 
Points in the point clouds are expressed in spherical coordinates wrt the source of the lidar. To generate a map we need cartesian coordiantes, so we want an *inverse sensor model* (spherical -> cartesian)

![[Screenshot 2020-11-13 at 09.41.08.png]]

## Sources of Error

Sources of noise:
- uncertainty in the time of arrival
- uncertainty in the orientation of the mirror
- Interaction with target (surface absorption, specural reflection, ...)
- variation of speed of light in different mediums 

These uncertainties are added to the sensor model as guassian noise, with selected covariance. 
![[Screenshot 2020-11-13 at 09.43.57.png]]

Motion Distorsion:
- Scan rate 5-20 Hz 
- This means that each patch of points is taken from a different position and orientation since the  speed of the vehicle is not neglectable wrt the angular rate of the scan. 




