# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program
[//]: # (Image References)

[image1]: ./content/kinematic_model.jpg  "Kinematic Model"

### The Model

Model predictive control is used to determine the vehicle's actuator input. The model calculates the vehicles trajectory one second away and then uses a non-linear optimizer to find the set of actuator values that will minimize the difference between the predicted state and the reference trajectory. 

Kinematic model equations are used as constaints to predict the vehicles future state. The equations are as follows:

![alt text][image1]

The inputs to the optimizer are 

Deviation from reference state (cross track error or cte)
Error in steering angle 
Error in velocity
Magnitude of actuator input 
Number of subsequent actuator inputs (To make the vehicle drive more smoothly)

The goal for the optimizer is to minimize these values. They may be scaled by independent weights in order to provide some tunability.

### Timestep Length and Elapsed Duration (N & dt)
The number of timesteps (N) of 10 was chosen as a compromise between number of waypoints and computation time. Since the program is intended for real time use, and the time complexity scales with N, it is necessary to keep a reasonable N size. A step size of 0.1 was found to provide sufficient resolution for the prediction. An N of 20 and dt of .5 was tried, but the increased delay caused the car to oscillate around the reference trajectory. 

### Polynomial Fitting and MPC Preprocessing
A polynomial is fit to predicted waypoints and it is displayed during the simulation. 

### Model Predictive Control with Latency
In order to compensate for latency, the current state that is fed into the update equations is first estimated using Kimeatic equations with a dt roughly equal to the expected latency. Estimating the current velocity using throttle input was attempted, but this did not seem to improve the results, so it was removed.