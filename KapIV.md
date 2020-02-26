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
