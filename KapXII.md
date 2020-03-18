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
    <img src="https://drive.google.com/uc?export=view&id=19wVnSOuhlOEECWJ5RL1qdnYI9Y4YSf-n" alt="note_1.png">
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

## Implementation of Planar Form Closure Test （by First-Order Analysis）
This implementation is done in MATLAB, the algorithm is adopted from the textbook. The basic idea behind the first-order analysis using the linear solver is that, if $$\text{pos}(\cup_i \mathscr{F}_i)$$ contains the origin $$[0\; 0\; 0]^T$$, then every possible wrench $$\mathscr{F}$$ will be spanned by this positive span, i.e., the body is in form closure.

```Matlab
%% This script takes in a list of support points and the contact normal in
%  radians measured from the positive x-axis, in the following form:
%  |    x    |      y     |   angle of contact normal from positive x axis|
%  |    0    |      0     |                  1.5708                       |
%  is a support point at origin with its contact normal pointing in the
%  direction of positive y-axis.
%  If the planar body IS in form closure by first-order anaylsis, this
%  script returns 1; if not, returns 0.
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020.03.17
%  For personal interests


%% Use this part to test the function
% Alternative way of getting input support point list:
% pointList = readmatrix('Input_points.csv');
%% Test 1 (see the corresponding screenshot for detail)
pointList1 = [[1 0 pi/2];
             [3 0 pi/2];
             [4 4  pi ];
             [0 6 -1.19029]];
IsClosure1 = FormClosureFirstOrder(pointList1);

%% Test 2
pointList2 = [[1 0 pi/2];
             [3 0 pi/2];
             [4 4  pi ];
             [0 6 -pi/4]];
IsClosure2 = FormClosureFirstOrder(pointList2);

%% Function define below
function IsClosure = FormClosureFirstOrder(pointList)
    N = size(pointList,1);
    f = ones(1,N);
    A = -1 .* eye(N);
    b = -1 .* f;
    F = [];
    for i = 1:N
        p = [pointList(i,1) pointList(i,2)];
        theta = pointList(i,3);
        n = [cos(theta) sin(theta)];
        m_iz = cross([p 0],[n 0]);
        F = [F [m_iz(3); n(1); n(2)]];
    end
    Aeq = F;
    beq = [0 0 0];
    k = linprog(f,A,b,Aeq,beq);
    if isempty(k)
        fprintf('The body is not in form closure by first-order analysis.\n');
        IsClosure = false;
    else 
        fprintf('The body is in form closure by first-order analysis.\n');
        IsClosure = true;
    end
end
```

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1Yv8yIH_ltmqXTw42Dl-Uo0ux40MoQMBc" alt="2TestCase.png">
</p>

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

> _**Exercise 12.6**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by finger 1. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1PcDEU4MoQViM183384hB8JaLpTVsBo6K" alt="fig_4.png">
</p>

> _**Exercise 12.7**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1 and 2. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=15VyRmFY7WkIeXlQpJgf82lmiVu2m_ITh" alt="fig_5.png">
</p>

> _**Exercise 12.8**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 2 and 3. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1FJLphy3EFFTXi8MGNOuAfDw35fc2co-V" alt="fig_6.png">
</p>

> _**Exercise 12.9**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1 and 5. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=11ayN9K-MrXFeveBebfLFBVSSzSQCclv4" alt="fig_7.png">
</p>

> _**Exercise 12.10**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1, 2, and 3.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1ZnCUHNvy21xonxvz_fpfEEAdlhHTjD-S" alt="fig_8.png">
</p>

> _**Exercise 12.11**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1, 2, and 4.


<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=19ZRNFS2KdfG9WmX1iItMsZ_9Okco7YPs" alt="fig_9.png">
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
