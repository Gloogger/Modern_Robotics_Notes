---
layout: post
mathjax: true
title: 'Kapitel IV: Forward Kinematics'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapIII.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapV.html';">Next</button></span>
</p>

## Useful Notes and Equations
For a comprehensive and thorough summary of the theory, check MuChenSun's wonderful note [here](https://muchensun.github.io/ModernRoboticsCourseNotes/ch3.html).

My own notes:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1wYI4DR4fpYST-s8QQRrNHSIwlt-jSAH_" alt="note_1.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1KYGsDUZOsULj4aBQVtI3rRhhfUzhhlPD" alt="note_2.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1zO-rZXdRBeAw1S9dykbHKAfGuKNKlksR" alt="note_3.png">
</p>


### Useful Equations:

***

## Textbook Exercises Attempts
> _**Exercise 4.2**_ The RRRP SCARA robot of Figure 4.12 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}. For $$l_{0} = l_{1} = l_{2} = 1$$ and the joint variable values $$\theta = (0, \frac{\pi}{2}, -\frac{\pi}{2},1)$$, use both the **FKinSpace** and the **FKinBody** functions to find the end-effector configuration $$T\in \text{SE}(3)$$. Confirm that they agree with each other.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1blEwO2RB8wACjnu2BADidBofK30CJoya" alt="fig_1.png">
</p>

```
%% Exercise 4.2
% function T = FKinSpace(M, Slist, thetalist)
M = [1 0 0 0;
     0 1 0 2;
     0 0 1 1;
     0 0 0 1];
Slist = [[0; 0; 1; 0; 0; 0], ...
         [0; 0; 1; 1; 0; 0], ...
         [0; 0; 1; 2; 0; 0], ...
         [0; 0; 0; 0; 0; 1]];
Blist = [[0; 0; 1;-2; 0; 0], ...
         [0; 0; 1;-1; 0; 0], ...
         [0; 0; 1; 0; 0; 0], ...
         [0; 0; 0; 0; 0; 1]];
thetalist =[0; pi/2; -pi/2; 1];
T_Slist = FKinSpace(M, Slist, thetalist);
T_Blist = FKinBody(M, Blist, thetalist);
============================================
>> T_Slist

T_Slist =

     1     0     0    -1
     0     1     0     1
     0     0     1     2
     0     0     0     1

>> T_Blist

T_Blist =

     1     0     0    -1
     0     1     0     1
     0     0     1     2
     0     0     0     1
```

> _**Exercise 4.4 to 4.6 are already attempted in my notes**_

> _**Exercise 4.7**_ The PRRRRR spatial open chain of Figure 4.13 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=15-oKmA8JODSi2RdGF3Aqdy-dYuuT5Nie" alt="fig_2.png">
</p>

> _**Exercise 4.8**_ The spatial RRRRPR open chain of Figure 4.14 is shown in its zero position, with fixed and end-effector frames chosen as indicated. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1BYTUlCWXmCznw9wGBlHLTjKG0GVi9eby" alt="fig_3.png">
</p>

> _**Exercise 4.9**_ The spatial RRPPRR open chain of Figure 4.15 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1uy_pCNvLxXS7Y4K7rfN1U36S9R_RXycP" alt="fig_4.png">
</p>

> _**Exercise 4.10**_ The URRPR spatial open chain of Figure 4.16 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=19wIFmyPiu4dLMAKMeWT0S7x-2FygKbCP" alt="fig_5.png">
</p>

> _**Exercise 4.11**_ The spatial RPRRR open chain of Figure 4.17 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1sRQV5rZnI2A08wwGjg6-i950pw6P_8vD" alt="fig_6.png">
</p>

> _**Exercise 4.12**_ The RRPRRR spatial open chain of Figure 4.18 is shown in its zero position (all joints lie on the same plane). Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}. Setting $$\theta_{5}=\pi$$ and all other joint variables to zero, find $$T_{06}$$ and $$T_{60}$$.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1sChw6SoXCxBfs8VyaR1vYbxykL-pza1U" alt="fig_7.png">
</p>

Setting $$\theta_{5}=\pi$$ and all other joint variables to zero, we have
$$
\begin{align}
    \begin{split}
        T_{06} &= e^{[\mathscr{S}_{1}]\theta_{1}} e^{[\mathscr{S}_{2}]\theta_{2}} e^{[\mathscr{S}_{3}]\theta_{3}} e^{[\mathscr{S}_{4}]\theta_{4}} e^{[\mathscr{S}_{5}]\theta_{5}} M\\
        &= I I I I e^{[\mathscr{S}_{5}]\theta_{5}} M\\
        &= \begin{bmatrix}
            -1 & 0 & 0 & 0 \\
             0 & 0 &-1 & 5L \\
             0 &-1 & 0 & 5L \\
             0 & 0 & 0 & 1 \\
        \end{bmatrix} \begin{bmatrix}
            1 & 0 & 0 & L \\
            0 & 1 & 0 & (4+\sqrt{2}) L \\
            0 & 0 & 1 & -\sqrt{2} L \\
            0 & 0 & 0 & 1 \\
        \end{bmatrix} \\
    \end{split}
\end{align}
$$

```
%% Exercise 4.12
e_s5_theta5 = [-1 0 0 0;
                0 0 -1 5;
                0 -1 0 5;
                0 0 0 1];
M = [1 0 0 1;
     0 1 0 4+sqrt(2);
     0 0 1 -sqrt(2);
     0 0 0  1];
T_06 = e_s5_theta5 * M;
T_60 = inv(T_06);
==========================
>> T_06

T_06 =

   -1.0000         0         0   -1.0000
         0         0   -1.0000    6.4142
         0   -1.0000         0   -0.4142
         0         0         0    1.0000
         
>> T_60

T_60 =

   -1.0000         0         0   -1.0000
         0         0   -1.0000   -0.4142
         0   -1.0000         0    6.4142
         0         0         0    1.0000
```

> _**Exercise 4.13**_ The spatial RRRPRR open chain of Figure 4.19 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {0}, and the screw axes $$\mathscr{B}_{i}$$ in {b}.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=14dgRZzlvCoOj9T6kSCiwkC4A_LXC7_1N" alt="fig_8.png">
</p>

> _**Exercise 4.14**_ The RPH robot of Figure 4.20 is shown in its zero position. Determine the end-effector zero position configuration M, the screw axes $$\mathscr{S}_{i}$$ in {s}, and the screw axes $$\mathscr{B}_{i}$$ in {b}. Use both the _FKinSpace_ and the _FKinBody_ functions to find the end-effector configuration $$T\in \text{SE}(3)$$ when $$\theta = (\pi/2, 3, \pi)$$. Confirm that the results agree.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1yCHzYEvAT8ZqMElYCwmycoz1Eb9237wp" alt="fig_9.png">
</p>

```
%% Exercise 4.14
Slist = [[0; 0; 1; 4; 0; 0], ...
         [0; 0; 0; 0; 1; 0], ...
         [0; 0;-1;-6; 0; -0.1]];
M = [-1 0  0 0;
      0 1  0 6;
      0 0 -1 2;
      0 0  0 1];
thetalist = [pi/2; 3; pi];
Blist = [[0; 0;-1; 2; 0; 0], ...
         [0; 0; 0; 0; 1; 0], ...
         [0; 0; 1;-6; 0; 0.1]];
T_03_s = FKinSpace(M, Slist, thetalist);
T_03_b = FKinBody(M, Blist, thetalist);
=========================================
>> T_03_s

T_03_s =

   -0.0000    1.0000         0   -5.0000
    1.0000    0.0000         0    4.0000
         0         0   -1.0000    1.6858
         0         0         0    1.0000

>> T_03_b

T_03_b =

   -0.0000    1.0000         0    7.0000
    1.0000    0.0000         0    4.0000
         0         0   -1.0000    1.6858
         0         0         0    1.0000
```
**Question!** Still don't know why $$T_{03,s}$$ and $$T_{03,b}$$ aren't equal.

> _**Exercise 4.17**_ Figure 4.22 shows a snake robot with end-effectors at each end.
Reference frames {$$b_{1}$$} and {$$b_{2}$$} are attached to the two end-effectors, as shown.
> - (a) Suppose that end-effector 1 is grasping a tree (which can be thought of as “ground”) and end-effector 2 is free to move. Assume that the robot is in its zero configuration. Then $$T_{b_{1}b_{2}} \in \text{SE}(3)$$ can be expressed in the following product of exponentials form:
  $$T_{b_{1}b_{2}} = e^{\mathscr{S}_{1}\theta_{1}} e^{\mathscr{S}_{2}\theta_{2}} \dots e^{\mathscr{S}_{5}\theta_{5}} M$$. 
  Find $$S_{3}$$, $$S_{5}$$, and $$M$$.
> - (b) **Don't know how to do**.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1hpQVTnJqAgtRw-QeZcIehN-zEjc4tTBo" alt="fig_10.png">
</p>


***

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
