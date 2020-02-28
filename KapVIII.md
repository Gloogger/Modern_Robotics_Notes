---
layout: post
mathjax: true
title: 'Kapitel VIII: Trajectory Generation'
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

My own notes:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1rD6lfWDzWFAR3CN-GxD9cGoFu4NmHmdJ" alt="note_1.png">
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
        I_{\text{sphere},{b},} &= I_{\text{sphere},{c}} + m d^{2}\\
        &= 0.12566 + 31.4159 \cdot 0.2 \\
        &= 1.38230 \, \text{kgm}^{2} \\
    \end{split}
\end{align}
$$

Therefore, the two spheres have the following inertia matrix:

$$ I_{\text{sphere}} = 
\begin{bmatrix}
       2.76460 & 0 & 0 \\
       0 & 2.76460 & 0 \\
       0 & 0 & 0.25132 \\
\end{bmatrix}
$$

Finally, adding the two inertia matrices up gives the inertial matrix of the dumbbell:

$$ I_{\text{dumbbell}} =
\begin{bmatrix}
    2.77107 & 0 & 0 \\
    0 & 2.77107 & 0 \\
    0 & 0 & 0.252377 \\
\end{bmatrix}
$$

- (b)


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
