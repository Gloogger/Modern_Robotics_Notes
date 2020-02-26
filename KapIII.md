---
layout: post
mathjax: true
title: 'Kapitel III: Rigid Body Motions'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapII.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapIV.html';">Next</button></span>
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
For $$R = \text{Rot}(\hat{v}_{i},-90^{\circ})$$, if:
1. $$R^{\prime}_{ab} = RR_{ab}$$, then the rotation is about $$\hat{v}_{a}$$ (in frame {a});
2. $$R^{\prime}_{ab} = R_{ab}R$$, then the rotation is about $$\hat{v}_{b}$$ (in frame {b}).

Adjacent Common Subscripts Cancellation Rule: if two subscripts are adjacent and common, then they can be cancelled. 
This rule applies to the product of two rotation matrix and the product of one rotation matrix with a vector.

Matrix Exponentials
$$
\hat{\omega}\theta = \vec{v} = \frac{1}{\lvert v \rvert}
\begin{bmatrix}
    \frac{v_{1}}{\lvert v \rvert} \\
    \frac{v_{2}}{\lvert v \rvert} \\
    \frac{v_{3}}{\lvert v \rvert} \\
\end{bmatrix}
$$

Adjoint representation formula:
$$
\left[\text{Ad}_{T}\right] = \begin{bmatrix}
    R & 0 \\
    [p]R & 0 \\
\end{bmatrix} \in \mathbb{R}^{6\times6}
$$

***

## Textbook Exercises Attempts
> _**Exercise 3.1**_ In terms of the $$\hat{x}_{s}$$, $$\hat{y}_{s}$$, $$\hat{z}_{s}$$ coordinates of a fixed space frame {s}, the frame {a} has its $$\hat{x}_{a}$$-axis pointing in the direction (0,0,1) and its $$\hat{y}_{a}$$-axis pointing in the direction (−1,0,0), and the frame {b} has its $$\hat{x}_{b}$$-axis pointing in the direction (1, 0, 0) and its $$\hat{y}_{b}$$-axis pointing in the direction (0, 0, −1).
> - (a) Draw by hand the three frames, at different locations so that they are easy to see.
> - (b) Write down the rotation matrices $$R_{sa}$$ and $$R_{sb}$$.
> - (c) Given $$R_{sb}$$, how do you calculate $$R^{-1}_{sb}$$ without using a matrix inverse? Write down $$R_{sb}^{-1}$$ and verify its correctness using your drawing.
> - (d) Given $$R_{sa}$$ and $$R_{sb}$$, how do you calculate $$R_{ab}$$ (again without using matrix inverses)?Compute the answer and verify its correctness using your drawing.
> - (e) Let R = $$R_{sb}$$ be considered as a transformation operator consisting of a rotation about $$\hat{x}$$ by $$−90^{\circ}$$. Calculate $$R_{1} = R_{sa}R$$, and think of $$R_{sa}$$ as a representation of an orientation, R as a rotation of $$R_{sa}$$, and $$R_{1}$$ as the new orientation after the rotation has been performed. Does the new orientation $$R_{1}$$ correspond to a rotation of $$R_{sa}$$ by $$−90^{\circ}$$ about the world-fixed $$\hat{x_{s}}$$-axis or about the body-fixed $$\hat{x}_{a}$$-axis? Now calculate $$R_{2} = RR_{sa}$$. Does the new orientation $$R_{2}$$ correspond to a rotation of $$R_{sa}$$ by $$−90^{\circ}$$ about the world-fixed $$\hat{x}_{s}$$-axis or about the body-fixed $$\hat{x}_a$$-axis?
> - (f) Use $$R_{sb}$$ to change the representation of the point $$p_{b} = (1,2,3)$$ (which is in {b} coordinates) to {s} coordinates.
> - (g) Choose a point p represented by $$p_{s} = (1, 2, 3)$$ in {s} coordinates. Calculate $$p^{\prime} = R_{sb}p_{s}$$ and $$p^{\prime\prime} = R^{T}_{sb}p_{s}$$. For each operation, should the result be interpreted as changing coordinates (from the {s} frame to {b}) without moving the point p or as moving the location of the point without changing the reference frame of the representation?
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

- (e) N.B. if $$R=\text{Rot}(\hat{u}_{i},\theta)$$ and R is pre-multiplied, that is, $$R^{\prime}=RR_{ab}$$, then $$\hat{u}_{i}$$ is $$\hat{u}_{a}$$; if R is post-multiplied, that is, $$R^{\prime\prime}=R_{ab}R$$, then $$\hat{u}_{i}$$ is $$\hat{u}_{b}$$.
  In this case, the rotation matrix is defined as, 
$$
R = R_{sb}=\text{Rot}(\hat{x}_{i},-90^{\circ})
$$
$$
\begin{align}
    \begin{split}
        R_{1} &= R_{sa} R \\
        &= R_{sa} \text{Rot}(\hat{x}_{a},-90^{\circ}) \\
        &= \begin{bmatrix}
            0 & -1 & 0 \\
            0 & 0 & -1 \\
            1 & 0 & 0 \\
        \end{bmatrix}
        \begin{bmatrix}
            1 & 0 & 0 \\
            0 & 0 & 1 \\
            0 & -1 & 0 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            0 & 0 & -1 \\
            0 & 1 & 0 \\
            1 & 0 & 0 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$
  Clearly, $$R_{1}$$ is $$R_{sa}$$ rotated $$-90^{\circ}$$ about $$\hat{x}_{a}$$.
$$
\begin{align}
    \begin{split}
        R_{2} &= R R_{sa} \\
        &= \text{Rot}(\hat{x}_{s},-90^{\circ})R_{sa}\\
        &= \begin{bmatrix}
            1 & 0 & 0 \\
            0 & 0 & 1 \\
            0 & -1 & 0 \\
        \end{bmatrix}
        \begin{bmatrix}
            0 & -1 & 0 \\
            1 & 0 & 0 \\
            0 & 0 & 1 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            0 & -1 & 0 \\
            1 & 0 & 0 \\
            0 & 0 & 1 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$
  Clearly, $$R_{2}$$ is $$R_{sa}$$ rotated $$-90^{\circ}$$ about $$\hat{x}_{s}$$.

- (f)
$$\begin{align}
    \begin{split}
        p_{s} &= R_{sb} p_{b} \\
        &= \begin{bmatrix}
            1 & 0 & 0 \\
            0 & 0 & 1 \\
            0 & -1 & 0 \\
        \end{bmatrix}
        \begin{bmatrix}
            1 \\
            2 \\
            3 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            1 \\
            3 \\
            -2 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$

- （g）
$$
\begin{align}
    \begin{split}
        p^{\prime} &= R_{sb}p_{s}\\
        &= \begin{bmatrix}
            1 \\
            3 \\
            -2 \\
        \end{bmatrix} \text{(from (f))}\\
        p^{\prime\prime} &= R^{T}_{sb}p_{s}\\
        &= R_{bs}p_{s} \\
        &= \begin{bmatrix}
            1 & 0 & 0 \\
            0 & 0 & -1 \\
            0 & 1 & 0 \\
        \end{bmatrix}
        \begin{bmatrix}
            1 \\
            2 \\
            3 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            1 \\
            -3 \\
            2 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$

$$p^{\prime}$$ should be interpreted as moving the location of the point without changing the reference frame. The reason is that the two adjacent subscripts are $$b$$ and $$s$$, which aren't common and therefore cannot be canceled. $$p^{\prime\prime}$$ should be interpreted as changing coordinates from {s} to {b} frame without moving the point $$p$$. The reason is that the two adjacent subscripts are common and can therefore be canceled. This cancellation indicates coordinate change.

- (h) 
$$
\begin{align}
    \begin{split}
        \omega_{a} &= R_{as}\omega_{s} = R_{sa}^{T}\omega_{s}\\
        &= \begin{bmatrix}
            0 & 0 & 1 \\
            -1 & 0 & 0 \\
            0 & -1 & 0 \\
        \end{bmatrix}
        \begin{bmatrix}
            3 \\
            2 \\
            1 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            1 \\
            -3 \\
            -2 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$

- (i) Because $$\text{tr}(R_{sa}) = 0 + 0 + 0 = 0$$, we should use the third general solution (see my own scribbling note).
  By the general formula, we have
$$
\begin{align}
    \begin{split}
        \theta &= \cos^{-1}\left( \frac{\text{tr}(R)-1}{2} \right) = \cos^{-1}\left(-\frac{1}{2}\right) \\
        &= \frac{2\pi}{3} \in [0,\pi) \\
    \end{split}
\end{align}
$$
With $$\theta$$, $$[\hat{\omega}]$$ can be obtained using formula as well,
$$
\begin{align}
    \begin{split}
        [\hat{\omega}] &= \frac{1}{2\sin(\frac{2\pi}{3})}(R-R^{T}) \\
        &= \frac{\sqrt{3}}{3}(R_{sa}-R_{as}) \\
        &= \frac{\sqrt{3}}{3}\left( 
            \begin{bmatrix}
                0 & -1 & 0 \\
                0 & 0 & -1 \\
                1 & 0 & 0 \\
            \end{bmatrix}
            -
            \begin{bmatrix}
                0 & 0 & 1 \\
                -1 & 0 & 0 \\
                0 & -1 & 0 \\
            \end{bmatrix}
        \right) \\
        &= \frac{\sqrt{3}}{3}\begin{bmatrix}
            0 & -1 & -1 \\
            1 & 0 & -1 \\
            1 & 1 & 0 \\
        \end{bmatrix}\\
    \end{split}
\end{align}
$$
By utilizing the equation $$R=e^{[\hat{\omega}]\theta}$$, we can find,
$$
\begin{align}
    \begin{split}
        \omega_{1} &= \frac{r_{32}-r_{23}}{2s_{\theta}} \\
        &= \frac{1-(-1)}{2s_{120^{\circ}}} = \frac{\sqrt{3}}{3}\cdot(2)\\
        \omega_{2} &= \frac{r_{13}-r_{31}}{2s_{\theta}} \\
        &= \frac{-1-1}{2s_{120^{\circ}}} = \frac{\sqrt{3}}{3}\cdot(-2)\\
        \omega_{3} &= \frac{r_{21}-r_{12}}{2s_{\theta}} \\
        &= \frac{1-(-1)}{2s_{120^{\circ}}} = \frac{\sqrt{3}}{3}\cdot(2)\\
    \end{split}
\end{align}
$$
To obtain the unit vector $$\hat{\omega}$$,
$$
\begin{align}
    \begin{split}
        \hat{\omega} &= \frac{1}{\lvert \omega \rvert} \begin{bmatrix}
            \omega_{1} \\
            \omega_{2} \\
            \omega_{3} \\ 
        \end{bmatrix} \\
        &= \frac{1}{2} \cdot \left( \frac{\sqrt{3}}{3} \begin{bmatrix}
            2 \\
            -2 \\
            2 \\
        \end{bmatrix}
        \right) 
        = \frac{\sqrt{3}}{3}
        \begin{bmatrix}
            1 \\
            -1 \\
            1 \\
        \end{bmatrix}\\
    \end{split} 
\end{align}
$$

- (j) Since $$\hat{\omega}\theta = (1, 2, 0)$$, we can find them separately by obtaining the unit vector:
$$
\hat{\omega}\theta = (1,2,0)^{T} = \sqrt{5}
\begin{bmatrix}
    \frac{1}{\sqrt{5}} \\
    \frac{2}{\sqrt{5}} \\
    0 \\
\end{bmatrix}
$$
Then it's obvious that we have $$\theta=\sqrt{5}$$ and $$\hat{\omega}=(\frac{1}{\sqrt{5}},\frac{2}{\sqrt{5}},0)^{T}$$.
Ipso facto, the skew symmetric form of $$\hat{\omega}$$ is, 
$$
[\hat{\omega}] = 
\begin{bmatrix}
    0 & 0 & \frac{2}{\sqrt{5}} \\
    0 & 0 & -\frac{1}{\sqrt{5}} \\
    -\frac{2}{\sqrt{5}} & \frac{1}{\sqrt{5}} & 0 \\
\end{bmatrix}
$$
The rotation is obtained as
$$
\begin{align}
    \begin{split}
        R &= e^{[\hat{\omega}]\theta} \\
        &= I + \sin(\theta)[\hat{\omega}] + (1-\cos{\theta})[\hat{\omega}]^{2}\\
        &= 
        \begin{bmatrix}
            -0.29 & 0.65 & 0.70 \\
            0.65 & 0.68 & -0.35 \\
            -0.70 & 0.35 & -0.62 \\
        \end{bmatrix}\\
    \end{split}
\end{align}
$$

> _**Exercise3.2**_ Let $$p$$ be a point whose coordinates are $$p=\left( \frac{1}{\sqrt{3}},-\frac{1}{\sqrt{6}},\frac{1}{\sqrt{2}}\right)$$ with respect to the fixed frame $$\hat{x}-\hat{y}-\hat{z}$$. Suppose that $$p$$ is rotated about the fixed-frame $$\hat{x}$$-axis by 30 degrees, then about the fixed-frame $$\hat{y}$$-axis by 135 degrees, and finally about the fixed-frame $$\hat{z}$$-axis by −120 degrees. Denote the coordinates of this newly rotated point by $$p^{\prime}$$.
> - (a) What are the coordinates $$p^{\prime}$$?
> - (b) Find the rotation matrix $$R$$ such that $$p^{\prime} = Rp$$ for the $$p^{\prime}$$ you obtained in (a).

- (a) Just use the formulae: $$p^{\prime} = \text{Rot}(\hat{z},-135^{\circ})\text{Rot}(\hat{y},135^{\circ})\text{Rot}(\hat{x},30^{\circ})p$$. The calculation and result are skipped here.

- (b) $$R = \text{Rot}(\hat{z},-135^{\circ})\text{Rot}(\hat{y},135^{\circ})\text{Rot}(\hat{x},30^{\circ})$$.

> _**Exercise 3.3**_ Suppose that $$p_{i} \in \mathbb{R}^{3}$$ and $$p^{\prime}_{i} \in \mathbb{R}^{3}$$ are related by $$p^{\prime}_{i} = Rp_{i},\text{where} i = 1, 2, 3$$, for some unknown rotation matrix $$R$$. Find, if it exists, the rotation $$R$$ for the three input–output pairs $$p_{i} \Rightarrow p^{\prime}_{i}$$, where: 
> $$
\begin{align}
    \begin{split}
        p_{1} &= (\sqrt{2}, 0, 2) \Rightarrow p^{\prime}_{1} = (0, 2, \sqrt{2}), \\
        p_{2} &= (1,1,-1) \Rightarrow p^{\prime}_{2} = (\frac{1}{\sqrt{2}},\frac{1}{\sqrt{2}},-\sqrt{2}), \\
        p_{3} &= (0, 2\sqrt{2}, 0) \Rightarrow p^{\prime}_{3} = (-\sqrt{2},\sqrt{2},-2) \\
    \end{split}
\end{align}
$$

Easy question. Since $$R\left[p_{1}^{T}, p_{2}^{T}, p_{3}^{T}\right] = \left[(p^{\prime}_{1})^{T}, (p^{\prime}_{2})^{T}, (p^{\prime}_{3})^{T}\right]$$, then $$R$$ can be found if the inverse of the term exists: $$R=\left[(p^{\prime}_{1})^{T}, (p^{\prime}_{2})^{T}, (p^{\prime}_{3})^{T}\right]\left[p_{1}^{T}, p_{2}^{T}, p_{3}^{T}\right]^{-1}$$.

> _**Exercise 3.5**_ Find the exponential coordinates $$\hat{\omega}\theta \in \mathbb{R}^{3}$$ for the SO(3) matrix
> $$ 
    \begin{bmatrix}
       0 & -1 & 0 \\
       0 & 0 & -1 \\
       1 & 0 & 0 \\
    \end{bmatrix}
$$

Find $$\theta$$ first by 
$$
\begin{align}
    \begin{split}
        1+2c_{\theta} &= \text{tr}(R)=0 \\
        \theta&= \frac{2\pi}{3} \\
    \end{split}
\end{align}
$$

Then find $$\hat{\omega}$$ by
$$
\begin{align}
    \begin{split}
        2s_{\theta} \begin{bmatrix}
            \omega_{1} \\
            \omega_{2} \\
            \omega_{3} \\
        \end{bmatrix} &=
        \begin{bmatrix}
            r_{32} - r_{23} \\
            r_{13} - r_{31} \\
            r_{21} - r_{12} \\
        \end{bmatrix} = \begin{bmatrix}
            0-(-1) \\
            0-1 \\
            0-(-1) \\
        \end{bmatrix} \\
        \hat{\omega} &= \frac{1}{ 2s_{\frac{2\pi}{3}} } \begin{bmatrix}
            1 \\
            -1 \\
            1 \\
        \end{bmatrix} = \frac{1}{\sqrt{3}}\begin{bmatrix}
            1 \\
            -1 \\
            1 \\
        \end{bmatrix} \\
    \end{split}
\end{align}
$$

The exponential coordinate $$\hat{\omega}\theta$$ can therefore be found as 
$$
\begin{align}
    \begin{split}
        \hat{\omega}\theta &= \left( \frac{2\pi}{3} \right) \left( \frac{\sqrt{3}}{3} \begin{bmatrix} 
            1 \\
            -1 \\
            1 \\
        \end{bmatrix}\right)\\
    \end{split}
\end{align}
$$

> _**Exercise 3.16**_ In terms of the $$\hat{x}_{s}, \hat{y}_{s}, \hat{z}_{s}$$ coordinates of a fixed space frame {s}, frame {a} has its $$\hat{x}_{a}$$-axis pointing in the direction $$(0,0,1)$$ and its $$\hat{y}_{a}$$-axis pointing in the direction $$(−1, 0, 0)$$, and frame {b} has its $$\hat{x}_{b}$$-axis pointing in the direction $$(1, 0, 0)$$ and its $$\hat{y}_{b}$$-axis pointing in the direction $$(0, 0, −1)$$. The origin of {a} is at $$(3,0,0)$$ in {s} and the origin of {b} is at $$(0,2,0)$$ in {s}.
> - (a) Draw by hand a diagram showing {a} and {b} relative to {s}.
> - (b) Write down the rotation matrices $$R_{sa}$$ and $$R_{sb}$$ and the transformation matrices $$T_{sa}$$ and $$T_{sb}$$.
> - (c) Given $$T_{sb}$$, how do you calculate $$T^{−1}_{sb}$$ without using a matrix inverse? Write $$T_{sb}^{-1}$$ and verify its correctness using your drawing.
> - (d) Given $$T_{sa}$$ and $$T_{sb}$$, how do you calculate Tab (again without using matrix inverses)? Compute the answer and verify its correctness using your drawing.
> - (e) Let T = $$T_{sb}$$ be considered as a transformation operator consisting of a rotation about $$\hat{x}$$ by $$−90^{\circ}$$ and a translation along $$\hat{y}$$ by 2 units. Calculate $$T_{1} = T_{sa}T$$. Does $$T_{1}$$ correspond to a rotation and translation about $$\hat{x}_{s}$$ and $$\hat{y}_{s}$$, respectively (a world-fixed transformation of $$T_{sa}$$), or a rotation and translation about $$\hat{x}_{a}$$ and $$\hat{y}_{a}$$, respectively (a body-fixed transformation of $$T_{sa}$$)? Now calculate $$T_{2} = TT_{sa}$$. Does $$T_{2}$$ correspond to a body-fixed or world-fixed transformation of $$T_{sa}$$?
> - (f) Use $$T_{sb}$$ to change the representation of the point $$p_{b} = (1,2,3)$$ in {b} coordinates to {s} coordinates.
> - (g) Choose a point $$p$$ represented by $$p_{s} = (1, 2, 3)$$ in {s} coordinates. Calculate $$p^{\prime} = T_{sb}p_{s}$$ and $$p^{\prime\prime} = T^{-1}_{sb}p_{s}$$. For each operation, should the result be interpreted as changing coordinates (from the {s} frame to {b}) without moving the point p, or as moving the location of the point without changing the reference frame of the representation?
> - (h) A twist $$\mathscr{V}$$ is represented in {s} as $$\mathscr{V}_{s} = (3, 2, 1, −1, −2, −3)$$. What is its representation $$\mathscr{V}_{a}$$ in frame {a}?
> - (i) By hand, calculate the matrix logarithm $$[\mathscr{S}]\theta$$ of $$T_{sa}$$. (You may verify your answer with software.) Extract the normalized screw axis $$\mathscr{S}$$ and rotation amount $$\theta$$. Find a $$\{q,\hat{s},h\}$$ representation of the screw axis. Redraw the fixed frame {s} and in it draw $$\mathscr{S}$$.
> - (j) Calculate the matrix exponential corresponding to the exponential coordinates of rigid-body motion $$\mathscr{S}\theta = (0, 1, 2, 3, 0, 0)$$. Draw the corresponding frame relative to {s}, as well as the screw axis $$\mathscr{S}$$.

- (a)
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1P7FJ5s9Gjs-BK9uVYKnJgKMuEN2YpR7S" alt="fig_3_8.png" width="500">
</p>

- (b) 
$$ R_{sa} = 
\begin{bmatrix}
     0 & -1 & 0 \\
     0 & 0 & -1 \\
     1 & 0 & 0 \\
\end{bmatrix},
R_{sb} = 
\begin{bmatrix}
     1 & 0 & 0 \\
     0 & 0 & 1 \\
     0 & -1 & 0 \\
\end{bmatrix},
T_{sa} = 
\begin{bmatrix}
     0 & -1 & 0 & 3 \\
     0 & 0 & -1 & 0 \\
     1 & 0 & 0 & 0 \\
     0 & 0 & 0 & 1 \\
\end{bmatrix},
T_{sb} = 
\begin{bmatrix}
     1 & 0 & 0 & 0 \\
     0 & 0 & 1 & 2 \\
     0 & -1 & 0 & 0 \\
     0 & 0 & 0 & 1 \\
\end{bmatrix}
$$

- (c)
$$
\begin{align}
    \begin{split}
        T_{sb}^{-1} &= 
        \begin{bmatrix}
            R_{sb} & p_{b} \\
            0 & 1 \\
        \end{bmatrix} =
        \begin{bmatrix}
            R_{sb}^{T} & -R_{sb}^{T}p_{b} \\
            0 & 1 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            1 & 0 & 0 & 0 \\
            0 & 0 & -1 & 0 \\
            0 & 1 & 0 & -2 \\
            0 & 0 & 0 & 1 \\
        \end{bmatrix} \\
    \end{split}
\end{align}
$$

- (d)
$$
\begin{align}
    \begin{split}
        T_{ab} &= T_{as}T_{sb} \\
        &= T_{sa}^{-1}T_{sb}\\
        &= \begin{bmatrix}
            R_{sa}^{T} & -R_{sa}^{T}p_{a} \\
            0 & 1 \\
        \end{bmatrix} T_{sb}\\
        &= \begin{bmatrix}
            0 & 0 & 1 & 0 \\
            -1 & 0 & 0 & -3 \\
            0 & -1 & 0 & 0 \\
            0 & 0 & 0 & 1 \\
        \end{bmatrix}\\
    \end{split}
\end{align}
$$

- (e)
$$
\begin{align}
    \begin{split}
        T_{1} &= T_{sa}T\\
        &=\begin{bmatrix}
            0 & 0 & -1 & 1 \\
            0 & 1 & 0 & 0 \\
            1 & 0 & 0 & 0 \\
            0 & 0 & 0 & 1 \\
        \end{bmatrix}\\
    \end{split}
\end{align}
$$

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1XRX7ciAFVStfqXI3tyk8wBFA_KbZn_OQ" alt="fig_3_9.png" width="500">
</p>

By the drawing above, $$T_{1}$$ corresponds to a rotation about $$\hat{x}_{a}$$ of $$-90^{\circ}$$ plus a translation about $$\hat{y}_{a}$$ of $$2$$ units. This behaviour is predicted by the adjacent subscripts cancellation rule.

$$
\begin{align}
    \begin{split}
        T_{2} &= TT_{sa}\\
        &=\begin{bmatrix}
            0 & -1 & 0 & 3 \\
            1 & 0 & 0 & 2 \\
            0 & 0 & 1 & 0 \\
            0 & 0 & 0 & 1 \\
        \end{bmatrix}\\
    \end{split}
\end{align}
$$

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1BvEo-WgZTLpNTJkHxm_NY6ddlbCElJRQ" alt="fig_3_10.png" width="500">
</p>

By the drawing above, $$T_{2}$$ corresponds to a rotation about $$\hat{x}_{s}$$ of $$-90^{\circ}$$ plus a translation about $$\hat{y}_{s}$$ of $$2$$ units. This behaviour is predicted by the adjacent subscripts cancellation rule.

- (f)
$$
p_{s} = T_{sb}p_{b}= \begin{bmatrix}
    1 \\
    5 \\
    -2 \\
    1 \\
\end{bmatrix}
$$

Truncate the meaningless last row of the vector, we have

$$
p_{s} = \begin{bmatrix}
    1 \\
    5 \\
    -2 \\
\end{bmatrix}
$$

- (g) From part (f) we have,
$$
p^{\prime} = \begin{bmatrix}
    1 \\
    5 \\
    -2 \\
\end{bmatrix}
$$

$$p^{\prime}$$ should be interpreted as moving the location of point $$p_{s}$$ without changing the reference frame, because the two adjacent subscripts are not common and cannot be cancelled. 

$$
\begin{align}
    \begin{split}
        p^{\prime\prime} &= T_{sb}^{-1}p_{s}\\
        &= T_{bs}p_{s}\\
        &= \begin{bmatrix}
            1 \\
            -3 \\
            0 \\
        \end{bmatrix}\text{last row truncated.}\\
    \end{split}
\end{align}
$$

$$p^{\prime\prime}$$ should be interpreted as changing the reference frame from {s} to {a} of point $$p_{s}$$ without moving its location, because the two adjacent subscripts are common and can therefore be cancelled. 

- h) We know that $$\mathscr{V}_{a}=\left[\text{Ad}_{T_{as}}\right]\mathscr{V}_{s}$$. To find $$\text{Ad}_{T_{as}}$$, we need to find $$T_{as}$$ first,
$$
T_{as} = T_{sa}^{-1} = \begin{bmatrix}
    0 & 0 & 1 & 0 \\
    -1 & 0 & 0 & 3 \\
    0 & -1 & 0 & 0 \\
    0 & 0 & 0 & 1 \\
\end{bmatrix}
$$

Then, using the adjoint representation formula, we have
$$
\left[\text{Ad}_{T_{sa}}\right] = \begin{bmatrix}
    0 & 0 & 1 & \; & 0 & 0 & 0 \\
   -1 & 0 & 0 & \; & 0 & 0 & 0 \\
    0 &-1 & 0 & \; & 0 & 0 & 0 \\
    \,&\, &\, & \, &\, &\, &\, \\
    0 &-3 & 0 & \; & 0 & 0 & 1 \\
    0 & 0 & 0 & \; &-1 & 0 & 0 \\
    0 & 0 &-3 & \; & 0 &-1 & 0 \\
\end{bmatrix}
$$
Finally, we have 
$$
\mathscr{V}_{a} = \begin{bmatrix}
    1 \\
    -3 \\
    -2 \\
    -9 \\
    1 \\
    -1 \\
\end{bmatrix}
$$

- (i) This is a big chunk. Get Ready! The calculation is done on MATLAB.
We have found $$R_{sa}$$ in previous parts, now we need to find $$\theta$$ and $$[\hat{\omega}]$$. Because $$\text{tr}(R_{sa})=0$$, the third choice of the algorithm should be used. Therefore, we have

$$
\begin{align}
    \begin{split}
        \theta &= \cos^{-1}\left(\frac{\text{tr}(R_{sa})-1}{2}\right)=\frac{2\pi}{3}, \\
        [\hat{\omega}] &= \frac{1}{2\sin(\theta)}(R_{sa}-R_{sa}^{T}) = \frac{\sqrt{3}}{3} \begin{bmatrix}
            0 & -1 & -1 \\
            1 &  0 & -1 \\
            1 &  1 &  0 \\
        \end{bmatrix} \\
    \end{split}
\end{align}
$$

```
%% Exercise 3.16
%% (i)
R_sa = [0 -1  0;
        0  0 -1;
        1  0  0];
theta = acos(-1/2);
skew_omega = 1/(2*sin(theta)) .* (R_sa - transpose(R_sa));
```
Now we need to find $$v$$. From the formula we know that $$v=G^{-1}(\theta)p$$. Therefore, we need to first find $$G^{-1}(\theta)$$ using,
$$
\begin{align}
    \begin{split}
        G^{-1}(\theta) &= \frac{1}{\theta}I-\frac{1}{2}[\theta]+\left( \frac{1}{\theta} - \frac{1}{2}\cot\frac{\theta}{2} \right)[\omega]^{2} \\
        &= \begin{bmatrix}
            0.3516  &  0.2257  &  0.3516 \\
            -0.3516 &   0.3516 &   0.2257 \\
            -0.2257 &  -0.3516 &   0.3516 \\
        \end{bmatrix} \\
        v &= G^{-1}(\theta)p \\
        &= \begin{bmatrix}
            1.0548  \\
            -1.0548 \\
            -0.6772 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$

Since $$\omega$$ does not equal to 0, we know that $$\mathscr{S}$$ is just the normalized $$\mathscr{V}$$. Therefore we have
$$
\mathscr{S} = (0.5774, -0.5774, 0.5774, 1.0548, -1.0548, -0.6772) ^{T}
$$
The unit screw axis $$\hat{s}$$ is simply $$[0.5774, -0.5774, 0.5774]^{T}$$. Then find $$h=\frac{\hat{\omega}^{T}v}{\dot{\theta}}=0.8271$$.

```
I = eye(3);     
inv_G = 1/theta .* I - 1/2 .* skew_omega + ( 1/theta - 1/2 * cot(theta/2) ) .* skew_omega^2;
p = [3; 0; 0];
v = inv_G * p;
skew_S = [skew_omega v;
          0  0   0   0];
s_hat = [0.5774; -0.5774; 0.5774]; % by hand
omega_hat = s_hat;
h = transpose(omega_hat) * v;
```

Because in $$v=-\omega\times q + h \omega$$ there is a cross product, we need to solve a system of equations.
Rearrange the aforesaid equation as $$v-h\omega = -\omega\times q$$. Let $$q=(a,b,c)^{T}$$. Then we have,
$$
\begin{align}
    \begin{split}
        \begin{bmatrix}
            0.5773 \\
            -0.5773 \\
            -1.1548 \\
        \end{bmatrix} &= -\left\lvert \begin{array}{ccc}
            i & j & k \\
            0.5773 & -0.5773 & 0.5773 \\
            a & b & c \\
        \end{array}\right\rvert \\
    \end{split}
\end{align}
$$

$$
\left[ \begin{array}{ccc|c} 
            0 & 0.5773 & 0.5773 & 0.5773 \\ 
            -0.5773 & 0 & 0.5773 & -0.5773 \\
            -0.5773 & -0.5773 & 0 & -1.1548 \\
            \end{array} 
         \right] \Longrightarrow q = \begin{bmatrix}
            1 \\
            1 \\ 
            0 \\
         \end{bmatrix}
$$

At last, the screw axis represented in $$\{q,\hat{s},h\}$$ form is 

$$
\{q,\hat{s},h\} = \left\{\begin{bmatrix}
    1 \\
    1 \\
    0 \\
\end{bmatrix}, \begin{bmatrix}
    0.5774 \\
    -0.5774 \\
    0.5774 \\
\end{bmatrix}, 0.8271 \right\}
$$

- (j) The calculation steps are:
1. Because $$\mathscr{S}\theta=[\hat{\omega}\theta,\,v\theta]^{T}$$, we can find $$\hat{\omega}$$ and $$\theta$$.
2. Since $$e^{[\mathscr{S}]\theta}=\begin{bmatrix} e^{[\hat{\omega}]\theta} & G(\theta)v \\ 0 & 1 \\ \end{bmatrix}$$, we need to first find each term first.
3. Done!

Here is the actual MATLAB code:

```
%% (j)
s_theta = [0; 1; 2; 3; 0; 0];
omega_theta = [0; 1; 2];
v = [3; 0; 0];
theta = norm(omega_theta);
hat_omega = omega_theta ./ theta;
bracket_omega = [      0       -hat_omega(3)  hat_omega(2);
                  hat_omega(3)      0             0;
                 -hat_omega(2)      0             0;       ];
exp_omega_theta = eye(3) + sin(theta).* bracket_omega + (1-cos(theta)) ...
                  .* bracket_omega^2;
G = eye(3) .* theta + (1-cos(theta)).*bracket_omega + (theta-sin(theta))...
    .* bracket_omega^2;
exp_S_theta = [exp_omega_theta   G*v;
                0     0    0      1 ];
```
The final result is,
$$
e^{[\mathscr{S}]\theta} = \begin{bmatrix}
    -0.6173 &  -0.7037 &   0.3518 &   2.3602 \\
    0.7037 &  -0.2938 &   0.6469  &  4.3396 \\
   -0.3518 &   0.6469  &  0.6765 &  -2.1698 \\
         0   &      0   &      0  &  1.0000 \\
\end{bmatrix}
$$



***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
