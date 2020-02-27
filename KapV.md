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
    <img src="https://drive.google.com/uc?export=view&id=1kHnAA7EojWrDBG_zN_n7HMmwC7z0zzRO" alt="fig_2.png">
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
