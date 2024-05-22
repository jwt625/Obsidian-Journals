
# Chapter 1

Framework:
1. sensor data
2. visual odometry (VO)
3. backend filtering/optimization
4. loop closing: detect returning of previous scene/position and reduce drift
5. reconstruction


Motion of the robot (motion equation):
$$ \textbf{x}_k = f(\textbf{x}_{k-1}, \textbf{u}_k, \textbf{w}_k) $$
- $\textbf x_k$: position
- $\textbf u_k$: input commands
- $\textbf w_k$: noise
- subscript $1, ..., k$ are time steps.

Observation equation:
$$ \textbf{z}_{k,j } = h(\textbf{y}_j, \textbf{x}_k, \textbf{v}_{k,j}) $$
- $\textbf y_k$: landmark
- $\textbf z_{k,j}$: observation data
- $\textbf v_{k,j}$: noise
- $(k,j)\in \mathcal O$: set containing information at which pose the landmark was observed

Pose = position + rotation

Parametrization:
- In a plane: $\textbf x_k = [x_1, x_2, \theta]^T_k$.

The SLAM problem is a state estimation problem: how to solve the estimate $\textbf x$ and $\textbf y$ with the noisy control input $\textbf u$ and sensor reading $\textbf z$.

Classification
- linear/nonlinear: motion and observation equations
- Gaussian/non-Gaussian: noise property

Linear Gaussian (LG system) is the simplest, optimal estimation given by Kalman Filter (KF).
Nonlinear non-Gaussian: extended Kalman Filter (EKF) and nonlinear optimization.

Mainstream today (2016): graph optimization.

# Chapter 2

Skew-symmetric conversion:

![[Pasted image 20240515232304.png]]


Rodrigue's formula
$$ \textbf R = \cos\theta \textbf I + (1-\cos\theta) \textbf n \textbf n^T + \sin \theta \textbf n ^{\bigwedge} $$
from which we could derive  $\theta =\arccos((\text{tr}\textbf R-1)/2)$.



## Lie Algebra

Treat rotation matrix as parametrized by time $t$.
$$\textbf R(t)\textbf R(t)^T=\textbf I$$
It is easy to see $\dot {\textbf R}(t)\textbf R(t)^T$ is a skew-symmetric matrix, and thus it corresponds to a vector $\phi(t)$. As a result,
$$\dot{\textbf R}(t)=\phi(t)^{\bigwedge} \textbf R(t).$$
$$\textbf R(t)=\exp(\phi_0^{\bigwedge}t).$$
$\phi$ is called the tangent space near the origin of SO(3), or the Lie algebra.







# Chapter 6: visual odometry
2024/05/22, 9:15 am

SIFT: Scale-Invariant Feature Transform: classic, heavy calculation, cannot achieve real-time (2016). Rarely used
FAST keypoint: fast. No descriptor
ORB: Oriented FAST and Rotated BRIEF (descriptor)

![[Pasted image 20240522091726.png]]

FAST: compare brightness, different from neighboring pixels is likely a corner point
1. select a pixel p, get it brightness Ip
2. set a threshold, e.g. 20% of Ip
3. select 16 pixels on a circle with a radius of 3
4. of there are consecutive N points with brightness greater or smaller than Ip +- T, then p is a feature point. N is usually 12 (FAST-12). 9 and 11 are also common
5. repeat
Could check 1, 5, 9, 13 to quickly exclude non-corner points.
![[Pasted image 20240522092138.png]]


It has a scaling problem because of the fixed radius of 3. It also has no direction information. ORB adds the description of scale and rotation.

Image pyramid: downsampling image at different levels to obtain images with different resolutions.

