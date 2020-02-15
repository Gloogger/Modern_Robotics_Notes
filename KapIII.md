---
layout: post
mathjax: true
title: 'Kapitel III: Rigid Body Motions'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="alert('This is the first chapter!')">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='ch3.html';">Next</button></span>
</p>

## Useful Notes and Equations
For a comprehensive and thorough summary of the theory, check MuChenSun's wonderful note [here](https://muchensun.github.io/ModernRoboticsCourseNotes/ch3.html).

My own notes:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1DSVOJTZFy7FHh_PZEt5pvCj8gykDoQVq" alt="fig_3_1.png">
</p>
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=11WYN2LiH-tsEM9roD_sILGYw53YMNRuC" alt="fig_3_2.png">
</p>
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1Ple1x5e840aePlorG8lCpE-EKEx8cnYf" alt="fig_3_3.png">
</p>
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1hXkoADejzLHk8ZmXrbjG4BvwvYt5epME" alt="fig_3_4.png">
</p>
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1TWOMwscXP-kNIRIHHUHhaVyyvZZwx1RP" alt="fig_3_5.png">
</p>
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1x1tfihvmSfB7VnrFw1vkbwZYfiHVPdwV" alt="fig_3_6.png">
</p>

### Useful Equations:


***

## Textbook Exercises Attempts
> _**Exercise 3.1**_ In terms of the $$\hat{x}_{s}$$, $$\hat{y}_{s}$$, $$\hat{z}_{s}$$ coordinates of a fixed space frame {s}, the frame {a} has its $$\hat{x}_{a}$$-axis pointing in the direction (0,0,1) and its $$\hat{y}_{a}$$-axis pointing in the direction (−1,0,0), and the frame {b} has its $$\hat{x}_{b}$$-axis pointing in the direction (1, 0, 0) and its $$\hat{y}_{b}$$-axis pointing in the direction (0, 0, −1).
> - (a) Draw by hand the three frames, at different locations so that they are easy to see.
> - (b) Write down the rotation matrices $$R_{sa}$$ and $$R_{sb}$$.
> - (c) Given $$R_{sb}$$, how do you calculate $$R^{-1}_{sb}$$ without using a matrix inverse? Write down $$R_{sb}^{-1}$$ and verify its correctness using your drawing.
> - (d) Given $$R_{sa}$$ and $$R_{sb}$$, how do you calculate $$R_{ab}$$ (again without using matrix inverses)?Compute the answer and verify its correctness using your drawing.
> - (e) Let R = $$R_{sb}$$ be considered as a transformation operator consisting of a rotation about $$\hat{x}$$ by $$−90^{\circ}$$. Calculate $$R_{1} = R_{sa}R$$, and think of $$R_{sa}$$ as a representation of an orientation, R as a rotation of $$R_{sa}$$, and $$R_{1}$$ as the new orientation after the rotation has been performed. Does the new orientation $$R_{1}$$ correspond to a rotation of $$R_{sa}$$ by $$−90^{\circ}$$ about the world-fixed $$\hat{x_{s}}$$-axis or about the body-fixed $$\hat{x}_{a}$$-axis? Now calculate $$R_{2} = RR_{sa}$$. Does the new orientation $$R_{2}$$ correspond to a rotation of $$R_{sa}$$ by $$−90^{\circ}$$ about the world-fixed $$\hat{x}_{s}$$-axis or about the body-fixed $$\hat{x}_a$$-axis?
> - (f) Use $$R_{sb}$$ to change the representation of the point $$p_{b} = (1,2,3)$$ (which is in {b} coordinates) to {s} coordinates.
> - (g) Choose a point p represented by $$p_{s} = (1, 2, 3)$$ in {s} coordinates. Calculate $$p\prime = R_{sb}p_{s}$$ and $$p\prime\prime = R^{T}_{sb}p_{s}$$. For each operation, should the result be interpreted as changing coordinates (from the {s} frame to {b}) without moving the point p or as moving the location of the point without changing the reference frame of the representation?
> - (h) An angular velocity w is represented in {s} as ωs = (3, 2, 1). What is its representation ωa in {a}?
> - (i) By hand, calculate the matrix logarithm $$[\hat{\omega}]\theta$$ of $$R_{sa}$$. (You may verify your answer with software.) Extract the unit angular velocity $$\hat{\omega}$$ and rotation amount $$\theta$$. Redraw the fixed frame {s} and in it draw $$\hat{\omega}$$.
> - (j) Calculate the matrix exponential corresponding to the exponential coordinates of rotation $$\hat{\omega}\theta = (1, 2, 0)$$. Draw the corresponding frame relative to {s}, as well as the rotation axis $$\hat{\omega}$$.

- (a) As shown below:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1_zODky3y3I56t2ut5R0n7k3XEtbSKWoa" alt="fig_3_7.png" width="450">
</p>

- (b) By observation we have
$$
R_{sa}=
\begin{bmatrix}
    0 & -1 & 0 \\
    0 & 0 & -1 \\
    1 & 0 & 0 \\
\end{bmatrix},
R_{sb}=
\begin{bmatrix}
    1 & 0 & 0 \\
    0 & 0 & 1 \\
    0 & -1 & 0 \\
\end{bmatrix}
$$

- (c) 
$$
\begin{align}
    \begin{split}
        R_{sb}^{-1} &= R_{sb}^{T}\\
        &= R_{bs}\\
        &= 
        \begin{bmatrix}
            1 & 0 & 0 \\
            0 & 0 & -1 \\
            0 & 1 & 0 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$

- (d) 
$$
\begin{align}
    \begin{split}
        R_{ab} &= R_{as} R_{sb}\\
        &= R_{sa}^{T} R_{sb}\\
        &= \begin{bmatrix}
                0 & 0 & 1 \\
                -1 & 0 & 0 \\
                0 & -1 & 0 \\
            \end{bmatrix}
            \begin{bmatrix}
                1 & 0 & 0 \\
                0 & 0 & 1 \\
                0 & -1 & 0 \\
            \end{bmatrix} \\
        &= \begin{bmatrix}
            0 & -1 & 0 \\
            -1 & 0 & 0 \\
            0 & 0 & -1 \\
           \end{bmatrix}
    \end{split}
\end{align}
$$


***

Image hosting template:

```
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=" alt="Erklaerung">
</p>
```

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
