# What is the app about

Help you "set" a furniture with out having it

# Plane DETECTION

###### Graphics algorithm + Sensor to decide which points are on the same plane

For every two frame, we use Graphics algorithm to extract feature points



---

So what we want built is an app that help you "place" the furniture into your room without really having it

As the hardware nowadays perform better, we could use the knowledge of Computer Graphics to solve the problem, and it is augment reality



We live in a 3 dementional world, photos are two dementional representation of the 3D world

A 3D point (X, Y, Z) in could be transformed onto a 2D plane( cell phone screen ) 

We represent this transformation as a matrix:
$$
\begin{bmatrix} 
x \\
y \\
1 \\
w
\end{bmatrix}
=
\begin{bmatrix} 
1 & 0 & 0 & 0\\ 
0 & 1 & 0 & 0\\ 
1 & 0 & 1 & 0\\ 
0 & 0 & \frac{1}{d} & 0\\ 
\end{bmatrix}
\begin{bmatrix} 
X\\ 
Y\\ 
Z\\ 
1\\ 
\end{bmatrix}
$$
The middle matrix is called the perspective transform matrix, it project a 3D points onto a 2D plane![creen Shot 2018-03-26 at 2.14.19 P](/Users/Cancel/Desktop/Screen Shot 2018-03-26 at 2.14.19 PM.png)

[Computer Vision: Algorithms and Applications (September 3, 2010 draft)]



The simplist version of AR is to find a plane in the scene and put the virtual model, which is a set of rendered 3D points, on the plane . Once you move the camera, the virtual object is still there or once you move the object, the object is still on the plane 

So the first problem is how to a find the plane that we want to set the model

We use SIFT ( Scale invariant feature transform ) to find the feature point's on both frame. 



The first thing we want to know is how camera move between each frame, it turns out that two matrix R (rotation) and T (transform) represent the movement

Four pairs are need to calculate the R and T matrix, which are how the camera rotate and translate

![creen Shot 2018-03-26 at 2.39.17 P](/Users/Cancel/Desktop/Screen Shot 2018-03-26 at 2.39.17 PM.png)

Once the R and the T matrix are computed,  

For each pair of the same object points, we compute the two vectors's(from the camera to the screen point) intersectino point, which is the real coordinate of the object point, but usually the two vectors are not intersected because this model is based on pinhole camera and every step produce error, so rather than calculate the intersection, we calculate the point that has the minimum distance to both vectors

![creen Shot 2018-03-26 at 2.14.19 P](/Users/Cancel/Desktop/Screen Shot 2018-03-26 at 2.14.19 PM.png)

Then we solve for all the feature points' coordinates 

Then we find four points that are on the same plane:

ax + by + cz = d

We use this plane to set our virtual furniture, and the furniture is moveable only on this plane

Then for every frame, we caclulate the RT matrix and get the camera's coordinates and project the virtual furniture on the screen



In real development, cell phone has some useful sensors that help us reduce the iteration time during calculation and quickly solve the equations, the most useful ones are gyro (YEE-ro 陀螺仪), accelerometer and gravity sensor. Especially when calculating R and T matrix.

Actually those sensors are good at tracking rotations and accelerations but are not good at tracking uniform movement, so software algorithms make up these insufficient.

### Demo Time

### In the future

1. Depth detection(two camera for each pixel/SFM/Machine Learning)
2. Model build system



Save - photo - situation

- user input