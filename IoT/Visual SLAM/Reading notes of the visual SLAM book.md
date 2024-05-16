
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







