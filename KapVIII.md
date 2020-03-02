---
layout: post
mathjax: true
title: 'Kapitel VIII: Dynamics of Open Chains'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapVII.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapIX.html';">Next</button></span>
</p>

## Useful Notes and Equations
For a comprehensive and thorough summary of the theory, check MuChenSun's wonderful note [here](https://muchensun.github.io/ModernRoboticsCourseNotes/ch8.html).

My own useless notes:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1rD6lfWDzWFAR3CN-GxD9cGoFu4NmHmdJ" alt="note_1.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1dJFxAOmOdBuaR8Srisk2FyZPiZR4p1Fp" alt="note_2.png">
</p>

### Useful Equations:

***

## Textbook Exercises Attempts
> _**Exercise 8.2**_ Consider a cast iron dumbbell consisting of a cylinder connecting two solid spheres at either end of the cylinder. The density of the dumbbell is $$7500$$ $$\text{kg}/m^{3}$$. The cylinder has a diameter of $$4$$ cm and a length of $$20$$ cm. Each sphere has a diameter of $$20$$ cm.
> - (a) Find the approximate rotational inertia matrix $$\mathcal{I}_{b}$$ in a frame {b} at the center of mass with axes aligned with the principal axes of inertia of the dumbbell.
> - (b) Write down the spatial inertia matrix $$\mathcal{G}_{b}$$.

- (a)
See the diagram below:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1_vXdxfkMKCnMURUEImuCWfvWDoyml8Zh" alt="fig_1.png" width="500">
</p>
For the cylinder, we have

$$
\begin{align}
    \begin{split}
        m &= \pi r^{2} h \rho \\
        &= \pi \cdot 0.02^2 \cdot 0.2 \cdot 7500 \\
        &= 1.88500 \, \text{kgm}^{2} \\
        I_{xx} = I_{yy} &= \frac{m\left( 3r^{2} + h^{2} \right)}{12} \\
        &= 6.47168e-3 \, \text{kgm}^{2} \\
        I_{zz} = \frac{1}{2} m r^{2} \\
        &= 3.77e-4 \text{kgm}^{2} \\
    \end{split}
\end{align}
$$

Therefore,

$$ \mathcal{I}_{\text{cylinder}} = 
\begin{bmatrix}
    6.47168e-3 & 0 & 0 \\
    0 & 6.47168e-3 & 0 \\
    0 & 0 & 3.77e-4 \\
\end{bmatrix}
$$

For one sphere, we have

$$
\begin{align}
    \begin{split}
        a = b = c &= r = 0.1 \text{m} \\
        m = \rho V &= \rho \frac{4\pi a b c}{3} \\
        &= 31.41590 \, \text{kgm}^{2} \\
    \end{split}
\end{align}
$$

$$
\begin{align}
    \begin{split}
        I_{xx} = I_{yy} = I_{zz} &= \frac{2m r^{2}}{5} \\
        &= 0.12566 \, \text{kgm}^{2} \\
    \end{split}
\end{align}
$$

Because the inertia of the sphere is calculated in its own center frame {c}, we need to shift $$\hat{x}_{c}$$ and $$\hat{y}_{c}$$ to the center frame {b} (we don't need to touch $$\hat{z}_{c}$$ because it is aligned with $$\hat{z}_{b}$$). Using the parallel axis theorem $$I_{aa} = I_{bb} + m d ^{2}$$, we have 

$$
\begin{align}
    \begin{split}
        I_{\text{sphere, {b}}} &= I_{\text{sphere, {c}}} + m d^{2}\\
        &= 0.12566 + 31.4159 \cdot 0.2 \\
        &= 1.38230 \, \text{kgm}^{2} \\
    \end{split}
\end{align}
$$

Therefore, the two spheres have the following inertia matrix:

$$ \mathcal{I}_{\text{sphere}} = 
\begin{bmatrix}
       2.76460 & 0 & 0 \\
       0 & 2.76460 & 0 \\
       0 & 0 & 0.25132 \\
\end{bmatrix}
$$

Finally, adding the two inertia matrices up gives the inertial matrix of the dumbbell:

$$ \mathcal{I}_{\text{dumbbell}} =
\begin{bmatrix}
    2.77107 & 0 & 0 \\
    0 & 2.77107 & 0 \\
    0 & 0 & 0.252377 \\
\end{bmatrix}
$$

- (b) The total mass of the dumbbell is $$\sum m = 64.7168\,\text{kg}$$. Therefore, the spatial inertia matrix is,

$$ \mathcal{G}_{b} = 
\begin{bmatrix}
    \mathcal{I}_{dumbbell} & 0 \\
    0 & 64.7168 \cdot \begin{bmatrix}
       1 & 0 & 0 \\
       0 & 1 & 0 \\
       0 & 0 & 1 \\
    \end{bmatrix}
\end{bmatrix}
$$

> _**Exercise 8.15**_ Dynamics of the UR5 robot.
> - (b) Simulate the UR5 falling under gravity with acceleration $$g = 9.81 \text{m/s}^{2}$$ in the $$-\hat{z}_{s}$$-direction. The robot starts at its zero configuration and zero joint torques are applied. Simulate the motion for three seconds, with at least 100 integration steps per second. (Ignore the effects of friction and the geared rotors.)

See MATLAB code below:

```Matlab
%% Data Given in Section 4.2
M01 = [1, 0, 0, 0; 0, 1, 0, 0; 0, 0, 1, 0.089159; 0, 0, 0, 1];
M12 = [0, 0, 1, 0.28; 0, 1, 0, 0.13585; -1, 0, 0, 0; 0, 0, 0, 1];
M23 = [1, 0, 0, 0; 0, 1, 0, -0.1197; 0, 0, 1, 0.395; 0, 0, 0, 1];
M34 = [0, 0, 1, 0; 0, 1, 0, 0; -1, 0, 0, 0.14225; 0, 0, 0, 1];
M45 = [1, 0, 0, 0; 0, 1, 0, 0.093; 0, 0, 1, 0; 0, 0, 0, 1];
M56 = [1, 0, 0, 0; 0, 1, 0, 0; 0, 0, 1, 0.09465; 0, 0, 0, 1];
M67 = [1, 0, 0, 0; 0, 0, 1, 0.0823; 0, -1, 0, 0; 0, 0, 0, 1];
G1 = diag([0.010267495893, 0.010267495893,  0.00666, 3.7, 3.7, 3.7]);
G2 = diag([0.22689067591, 0.22689067591, 0.0151074, 8.393, 8.393, 8.393]);
G3 = diag([0.049443313556, 0.049443313556, 0.004095, 2.275, 2.275, 2.275]);
G4 = diag([0.111172755531, 0.111172755531, 0.21942, 1.219, 1.219, 1.219]);
G5 = diag([0.111172755531, 0.111172755531, 0.21942, 1.219, 1.219, 1.219]);
G6 = diag([0.0171364731454, 0.0171364731454, 0.033822, 0.1879, 0.1879, 0.1879]);
Glist = cat(3, G1, G2, G3, G4, G5, G6);
Mlist = cat(3, M01, M12, M23, M34, M45, M56, M67); 
Slist = [0,         0,         0,         0,        0,        0;
         0,         1,         1,         1,        0,        1;
         1,         0,         0,         0,       -1,        0;
         0, -0.089159, -0.089159, -0.089159, -0.10915, 0.005491;
         0,         0,         0,         0,  0.81725,        0;
         0,         0,     0.425,   0.81725,        0,  0.81725];
         
%% Define gravity and joint positions
g = [0; 0; -9.81];                  % gravity term
thetalist = [0; 0; 0; 0; 0; 0];     % home configuration
dthetalist = [0; 0; 0; 0; 0; 0];    % Originally stationary
dt = 0.01;                          % 100 steps per second
simuTime = 3;                       % Length of simulation in seconds
Ftipmat = zeros(simuT1/dt1, 6);     % simuT1/dt1 gives the total steps,
% since the XXmat function is defined as a Nxn matrix, in which the N 
% refers to steps and n refers to the joint variables
taumat = zeros(simuT1/dt1, 6);      % same as above

intRes = 10;   % Magic number for integration resolution, chosen arbitrarily.

%% Calculate thetamatrix
[thetamat, dthetamat] ...
         = ForwardDynamicsTrajectory(thetalist, dthetalist, taumat, g, ...
                                     Ftipmat, Mlist, Glist, Slist, dt, ...
                                     intRes);
                                     
%% Output trajectory as .csv file
writematrix(thetamat,'forSimulation.csv','Delimiter','comma')
% Use the .csv file in CoppeliaSim to visualize the motion
```


***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
