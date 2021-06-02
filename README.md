# imuFilter

This library contains a sensor fusion algorithm to combine the outputs of a 6-axis inertial measurement unit (IMU). It's based on a modified version of the Mahony filter that replaces the PI controller with something akin to a 2nd order low pass filter. The proportional term was removed and the integral term has been forced to decay in order to damp the system. The correction steps of each filter are shown below:

- Mahony:  
_integral += error.dt   
dtheta = theta_dot.dt + kp.error + ki.integral_  

- Modified Mahony:  
_integral += error.kp - integral.kc    
dtheta = theta_dot.dt + integral_  

The behavior of the modified filter is analogous to spring-mass system. Kp (stiffness) and Kc (damping) are related by the damping ratio Q which in this case is held constant. Doing so allows behavior of the filter to be controlled with a single parameter.  

The filter also uses a quaternion to encode rotations which makes it easy to perform coordinate transformations. These include:  
- Transfor a vector from the local frame to the global (and vice versa)
- Get unit vectors of the X, Y and Z axes in the local or global frame.

Since a 6-axis IMU has no absolute reference for heading (no magnetometer) there is a rountine to rotate the current heading (yaw angle) estimate. Basic vector operations are also included to easily implement a heading correction algorithm if one has an adequate sensor. 

For more information on the Mahony filter see these links:
- [IMU Data Fusing: Complementary, Kalman, and Mahony Filter](http://www.olliw.eu/2013/imu-data-fusing/#chapter23)
- [Mahony Filter](https://nitinjsanket.github.io/tutorials/attitudeest/mahony)
