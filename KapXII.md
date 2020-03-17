---
layout: post
mathjax: true
title: 'Kapitel XII: Grasping and Manipulation'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapXI.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapXIII.html';">Next</button></span>
</p>

## Useful Notes and Equations
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1DSVOJTZFy7FHh_PZEt5pvCj8gykDoQVq" alt="fig_3_1.png">
</p>


### Useful Equations:

* From twist to CoR:

$$\text{CoR}(\mathscr{V})=\left\{ \text{sgn}(\omega_z),\; \begin{bmatrix}
    x_c \\
    y_c \\
\end{bmatrix}
\right\}$$

$$
\begin{bmatrix}
 x_c \\
 y_c \\
\end{bmatrix} = \begin{bmatrix}
 -\frac{v_y}{\omega_z} \\
 \frac{v_x}{\omega_z} \\
\end{bmatrix}
$$


***

## Textbook Exercises Attempts
> _**Exercise 12.1**_ Prove that the impenetrability constraint (12.4) is equivalent to the constraint (12.7).

See my notes above.

> _**Exercise 12.2**_ Representing planar twists as centers of rotation.
> - (a) Consider the two planar twists $$\mathscr{V}_1 = (\omega_{z1}, v_{x1}, v_{y1})=(1,2,0)$$ and $$\mathscr{V}_2 = (\omega_{z2}, v_{x2}, v_{y2})=(1,0,-1)$$. Draw the corresponding CoRs in a planar coordinate frame, and illustrate pos$$(\{\mathscr{V}_1 , \mathscr{V}_2\})$$ as CoRs.
> - (b) Draw the positive span of $$\mathscr{V}_1 = (\omega_{z1}, v_{x1}, v_{y1})=(1,2,0)$$ and $$\mathscr{V}_2 = (\omega_{z2}, v_{x2}, v_{y2})=(-1,0,-1)$$ as CoRs.

- (a) Using the twist-to-CoR formula, we have 

$$\begin{bmatrix}
 x_{c,1}\\
 y_{c,1} \\
\end{bmatrix} = \begin{bmatrix}
 -\frac{0}{1} \\
 \frac{2}{1} \\
\end{bmatrix} = \begin{bmatrix}
 0 \\
 2 \\
\end{bmatrix}
$$

$$\begin{bmatrix}
 x_{c,2}\\
 y_{c,2} \\
\end{bmatrix} = \begin{bmatrix}
 -\frac{-1}{1} \\
 \frac{0}{1} \\
\end{bmatrix} = \begin{bmatrix}
 1 \\
 0 \\
\end{bmatrix}
$$

- (b)
$$\begin{bmatrix}
 x_{c,2}\\
 y_{c,2} \\
\end{bmatrix} = \begin{bmatrix}
 -\frac{-1}{-1} \\
 \frac{0}{-1} \\
\end{bmatrix} = \begin{bmatrix}
 -1 \\
 0 \\
\end{bmatrix}
$$

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1GGh8EwjWRbT61bfkjRcJdzo1YcKC4brt" alt="fig_1.png" width="500">
</p>

> _**Exercise 12.3**_ A rigid body is contacted at $$p=(1,2,3)$$ with a contact normal into the body $$\hat{n}=(0,1,0)$$. Write the constraint on the body’s twist $$\mathscr{V}$$ due to this contact.

By the impenetrability constraint, we have $$ \mathscr{F}^T ( \mathscr{V}_a - \mathscr{V}_b ) \leq 0 $$. Assume everything other than the body is stationary, $$\mathscr{V}_b = 0$$.

$$
\begin{align}
    \begin{split}
        \mathscr{F}^T &= \left( [p]\hat{n} \;\; \hat{n} \right)^T \\
        &= \left( \left( \begin{bmatrix}
            0 & -3 & 2 \\
            3 & 0 & -1 \\
            -2 & 1 & 0 \\
        \end{bmatrix} \begin{bmatrix}
            0 \\
            1 \\
            0 \\
        \end{bmatrix} \right)^T \;\; \begin{bmatrix} 
            0 \\
            1 \\
            0 \\
        \end{bmatrix}^T \right) \\
        &= \begin{bmatrix}
         -3 & 0 & 1 & 0 & 1 & 0 \\
        \end{bmatrix} \\
    \end{split}
\end{align}
$$

$$
\begin{align}
    \begin{split}
        \mathscr{F}^T \mathscr{V}_A &= \begin{bmatrix}
         -3 & 0 & 1 & 0 & 1 & 0 \\
        \end{bmatrix} \begin{bmatrix}
            \omega_x \\
            \omega_y \\
            \omega_z \\
            v_x \\
            v_y \\
            v_z \\
        \end{bmatrix} \\
        &= -3 \omega_x + \omega_z + v_y \leq 0 \\
    \end{split}
\end{align}
$$

> _**Exercise 12.4**_ A space frame $$\{ s \}$$ is defined at a contact between a stationary constraint and a body. The contact normal, into the body, is along the $$\hat{z}$$-axis of the $$\{ s \}$$ frame.
> - (a) Write down the constraint on the body’s twist $$\mathscr{V}$$ if the contact is a frictionless point contact.
> - (b) Write down the constraints on $$\mathscr{V}$$ if the contact is a point contact with friction.
> - (c) Write down the constraints on $$\mathscr{V}$$ if the contact is a soft contact.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1FJy47MkfiWZ9VO6deGbC0mfuh26uaTbz" alt="fig_2.png" width="400">
</p>

> _**Exercise 12.5_** Figure 12.29 shows five stationary “fingers” contacting an object. The object is in first-order form closure and therefore force closure. If we take away one finger, the object may still be in form closure. For which subsets of four fingers is the object still in form closure? Prove your answers using graphical methods.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1tNVmD-_iTatDHy7wcEoldg85_BM8GKZp" alt="fig_3.png">
</p>

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
