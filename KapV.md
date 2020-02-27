---
layout: post
mathjax: true
title: 'Kapitel V: Velocity Kinematics and Statics'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapIV.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapVI.html';">Next</button></span>
</p>

## Useful Notes and Equations
For a comprehensive and thorough summary of the theory, check MuChenSun's wonderful note [here](https://muchensun.github.io/ModernRoboticsCourseNotes/ch5.html).

My own notes:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1A8Ec1mKRcLSJNpC3ufkbW4cMtJEV5wsw" alt="note_1.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1DxzZfoKaJCzr38u5020W_cQ3biWiSt-U" alt="note_2.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1KAkKK1wnkGBDknWLTt2PkT7J9HXcF5VJ" alt="note_3.png">
</p>


### Useful Equations:

***

## Textbook Exercises Attempts
> _**Exercise 5.1**_ A wheel of unit radius is rolling to the right at a rate of 1 rad/s (see Figure 5.14; the dashed circle shows the wheel at $$t = 0$$).
> - (a) Find the spatial twist $$\mathscr{V}_{s}(t)$$ as a function of $$t$$.
> - (b) Find the linear velocity of the {b}-frame origin expressed in {s}-frame coordinates.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=17O-kHM4aTNNQocBkjoGZ98V7XOJrgLDD" alt="fig_1.png">
</p>

> _**Exercise 5.2**_ The 3R planar open chain of Figure 5.15(a) is shown in its zero position.
> - (a) Suppose that the last link must apply a wrench corresponding to a force of 5 N in the $$\hat{x}^{s}$$-direction of the {s} frame, with zero component in the $$\hat{y}_{s}$$-direction and zero moment about the $$\hat{z}_{s}$$-axis. What torques should be applied at each joint?
> - (b) Suppose that the last link must apply a force of 5 N in the yˆs-direction, with zero components in the other wrench directions. What torques should be applied at each joint?
  <p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1kHnAA7EojWrDBG_zN_n7HMmwC7z0zzRO" alt="fig_2.png" width="400">
  </p>
  
N.B. in figure part (a), the robot is not in its zero position!!!!

- (a) Since $$\tau = J_{s}^{T}(\theta) \cdot \mathscr{F}_{s}$$, we need to first find _SList_ and _thetalist_ and then use the function _JacobianSpace_ to find $$J_{s}^{T}(\theta)$$. $$\mathscr{F}_{s}$$ can be found by observation. The following MATLAB code is self-explanatory.

```
%% Exercise 5.2 

%  Part (a)
F_s = [0; 0; 0; 5; 0; 0];
Slist = [[0; 0; 1; 0; 0; 0], ...
         [0; 0; 1; 0;-1; 0], ...
         [0; 0; 1; 0;-2; 0]];
thetalist =[0; pi/4; 0];
J_s_T = transpose(JacobianSpace(Slist, thetalist));
tau = J_s_T * F_s;

%  Part (b)
F_s_b = [0; 0; 0; 0; 5; 0];
tau_b = J_s_T * F_s_b;
===================================================
>> tau_a

tau_a =

         0
         0
    3.5355

>> tau_b

tau_b =

         0
   -5.0000
   -8.5355
```

> _**Exercise 5.3**_ Answer the following questions for the 4R planar open chain of Figure 5.15(b).
> - (a) For the forward kinematics of the form
  $$T(\theta) = e^{[\mathscr{S}_{1}]\theta_{1}} e^{[\mathscr{S}_{2}]\theta_{2}} e^{[\mathscr{S}_{3}]\theta_{3}} e^{[\mathscr{S}_{4}]\theta_{4}} M$$
  write down $$M \in \text{SE}(2)$$ and each $$\mathscr{S}_{i} = (\omega_{zi}, v_{xi}, v_{yi}) \in \mathbb{R}^{3}$$.
> - (b) Write down the body Jacobian.
> - (c) Suppose that the chain is in static equilibrium at the configuration $$\theta_{1}=\theta_{2}=0, \theta_{3}=\pi/2, \theta_{4}=-\pi/2$$ and that a force $$f = (10,10,0)$$ and a moment $$m = (0,0,10)$$ are applied to the tip (both f and m are expressed with respect to the fixed frame). What are the torques experienced at each joint?
> - (d) Under the same conditions as (c), suppose that a force $$f = (-10,10,0)$$ and a moment $$m = (0,0,-10)$$, also expressed in the fixed frame, are applied to the tip. What are the torques experienced at each joint?
> - (e) Find all kinematic singularities for this chain.
  <p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1xi5UOFij9gfPSPXuuYrBo459qLJta0O_" alt="fig_3.png" width="400">
  </p>

- (a) $$M = \begin{bmatrix}
   1 & 0 & L_{1} + L_{2} + L_{3} + L_{4} \\
   0 & 1 & 0 \\
   0 & 0 & 1 \\
\end{bmatrix} \in \text{SE}(2)$$, $$\mathscr{S} = \begin{bmatrix}
   1 & 1 & 1 & 1 \\
   0 & 0 & 0 & 0 \\
   0 & -L_{1} & -(L_{1}+L_{2}) & -(L_{1}+L_{2}+L_{3}) \\
\end{bmatrix} \in \mathbb{R}^{3}$$.

- (b) I am too lazy to do that by hand. One of the online quizzes of this Coursera MOOC actually gives you the answer...
- Because I am using MATLAB to do this course, and MATLAB is quite impotent on symbolic manipulation, part (c) and (d) are skipped.
- (e) When $$\theta_{2}=\theta_{3}=\theta_{4}=n\pi, \,\text{where}\,n\in \mathbb{Z}^{+}$$.

> _**Exercise 5.7**_ The RRP robot in Figure 5.19 is shown in its zero position.
> - (a) Write down the screw axes in the space frame. Evaluate the forward kinematics when $$\theta = (90^{\circ}, 90^{\circ}, 1)$$. Hand-draw or use a computer to show the arm and the end-e↵ector frame in this configuration. Obtain the space Jacobian $$J_{s}$$ for this configuration.
> - (b) Write down the screw axes in the end-effector body frame. Evaluate the forward kinematics when $$\theta = (90^{\circ}, 90^{\circ}, 1)$$ and confirm that you get the same result as in part (a). Obtain the body Jacobian $$J_{b}$$ for this configuration.
  <p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1aGs66Y9QodHuZXa16CpKxgfCoOS8ssnK" alt="fig_4.png" width="400">
  </p>

- (a)

***

Image hosting template:

```
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=" alt="Erklaerung">
</p>
```

```
$$
\begin{align}
    \begin{split}
    \end{split}
\end{align}
$$


$$
\begin{bmatrix}
       
\end{bmatrix}
$$
```

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
