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
    <img src="https://drive.google.com/uc?export=view&id=11LOT9CPEYguFDBbOLwN9eR1s2kWECX9x" alt="note_1.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1tTZaF6M-sdCEgs1vJ39RE87D680DRJPi" alt="note_2.png">
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
    <img src="https://drive.google.com/uc?export=view&id=1PcDEU4MoQViM183384hB8JaLpTVsBo6K" alt="fig_4.png" width="400">
</p>

> _**Exercise 12.7**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1 and 2. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=15VyRmFY7WkIeXlQpJgf82lmiVu2m_ITh" alt="fig_5.png" width="400">
</p>

> _**Exercise 12.8**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 2 and 3. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1FJLphy3EFFTXi8MGNOuAfDw35fc2co-V" alt="fig_6.png" width="500">
</p>

> _**Exercise 12.9**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1 and 5. Label the feasible CoRs with their contact labels.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=11ayN9K-MrXFeveBebfLFBVSSzSQCclv4" alt="fig_7.png">
</p>

> _**Exercise 12.10**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1, 2, and 3.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1ZnCUHNvy21xonxvz_fpfEEAdlhHTjD-S" alt="fig_8.png" width="400">
</p>

> _**Exercise 12.11**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1, 2, and 4.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=19ZRNFS2KdfG9WmX1iItMsZ_9Okco7YPs" alt="fig_9.png" width="500">
</p>

> _**Exercise 12.12**_ Draw the set of feasible twists as CoRs when the triangle of Figure 12.29 is contacted only by fingers 1, 3, and 5.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1HDGcSvx76MJ8yPTESAQfmEEeVx1Xp3Ym" alt="fig_10.png" width="450">
</p>

> _**Exercise 12.13**_ Refer again to the triangle of Figure 12.29.
> - (a) Draw the wrench cone from contact 5, assuming a friction angle $$\alpha = 22.5^{\circ}$$ (a friction coefficient $$\mu = 0.41$$), using moment labeling.
> - (b) Add contact 2 to the moment-labeling drawing. The friction coefficient at contact 2 is $$\mu = 1$$.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1kocJB37YALWHeyJ3AwwPcjDxLJV_Op19" alt="fig_11.png" width="500">
</p>

> _**Exercise 12.14**_ Refer again to the triangle of Figure 12.29. Draw the moment-labeling region corresponding to contact 1 with $$\mu = 1$$ and contact 4 with $$\mu = 0$$.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=14aFHIwqQCyV6XRLdvG0yQPrLU3ElJyli" alt="fig_12.png" width="400">
</p>

> _**Exercise 12.15**_ The planar grasp of Figure 12.30 consists of five frictionless point contacts. The square’s size is $$4 \times 4 $$.
> - (a) Show that this grasp does not yield force closure.
> - (b) The grasp of part (a) can be modified to yield force closure by adding one frictionless point contact. Draw all the possible locations for this contact.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1CwXr1kfz2tODs6ewepg8Ieo65Ek67RGY" alt="fig_13.png">
</p>

> _**Exercise 12.16**_ Assume the contacts shown in Figure 12.31 are frictionless point contacts. Determine whether the grasp yields force closure. If it does not, how many additional frictionless point contacts are needed to construct a force closure grasp?

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1-71yzEh_li8HtMheAx41gNJ_Ipp_nNAe" alt="fig_14.png">
</p>

> _**Exercise 12.17**_ Consider the L-shaped planar object of Figure 12.32.
> - (a) Suppose that both contacts are point contacts with friction coefficient $$\mu = 1$$. Determine whether this grasp yields force closure.
(b) Now suppose that point contact 1 has friction coecient μ = 1, while point contact 2 is frictionless. Determine whether this grasp yields force
closure.
> - (c) The vertical position of contact 1 is allowed to vary; denote its height by $$x$$. Find all positions $$x$$ such that the grasp is force closure with $$\mu = 1$$ for contact 1 and $$\mu = 0$$ for contact 2.
g

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1bwOg_Y51XeEc_z5JHHxU7-pkRJsA09e7" alt="fig_15_a.png" width="500">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=15alV9T8kNoOZ5P4jWwkqnyoti3SsjaV8" alt="fig_15_b.png" width="500">
</p>

- (c) It seems like the top-right '++++' area cannot be eliminated in the current grasp. Therefore no position is able to generate a force-closure grasp.

> _**Exercise 12.18**_ A square is restrained by three point contacts as shown in Figure 12.33: $$f_1$$ is a point contact with friction coefficient $$\mu$$, while $$f_2$$ and $$f_3$$ are frictionless point contacts. If $$c = \frac{1}{4}$$ and $$h = \frac{1}{2}$$, find the range of values of $$\mu$$ such that grasp yields force closure.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1Q4aEvhZUMyxqTRBXMPLfv67Ks4fzMi6w" alt="fig_16.png">
</p>

> _**Exercise 12.19**_
> - (a) For the planar grasp of Figure 12.34(a), assume contact C is frictionless, while the friction coecient at contacts A and B is $$\mu = 1$$. Determine whether this grasp is force closure.
> - (b) For the planar grasp of Figure 12.34(b), assume contacts A and B are frictionless, while contact C has a friction cone of half-angle $$\beta$$. Find the range of values of $$\beta$$ that makes this grasp force closure.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1-WDe7YAjWqMJCeAR-ecu-MvuKqgBmd5t" alt="fig_17_a.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1xPpPfAL-4oyW_9qYiNRT4ODUsDJ0JioX" alt="fig_17_b.png">
</p>

> _**Exercise 12.23**_ A frictionless finger begins pushing a box over a table (Figure 12.36). There is friction between the box and the table, as indicated in the figure. There are three possible contact modes between the box and the table: either the box slides to the right flat against the table, or it tips over at the right lower corner, or it tips over that corner while the corner also slides to the right. Which actually occurs? Assume a quasistatic force balance and answer the following questions.
> - (a) For each of the three contact modes, draw the moment-labeling regions corresponding to the table’s friction cone edges active in that contact mode.
> - (b) For each moment-labeling drawing, determine whether the pushing force plus the gravitational force can be quasistatically balanced by the support forces. From this, determine which contact mode actually occurs.
> - (c) Graphically show a different support-friction cone for which the contact mode is different from your solution above.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1ZSh-ZQX_hwbd5wfJaEZxjDmDx3OBjmYf" alt="fig_18.png" width="500">
</p>

> _**Exercise 12.24**_ In Figure 12.37 body 1, of mass m1 with center of mass at $$(x_1,y_1)$$, leans on body 2, of mass $$m_2$$ with center of mass at $$(x_2,y_2)$$. Both are supported by a horizontal plane, and gravity acts downward. The friction coefficient at all four contacts (at $$(0, 0)$$, at $$(x_L , y)$$, at $$(x_L , 0)$$, and at $$(x_R, 0)$$) is $$\mu > 0$$. We want to know whether it is possible for the assembly to stay standing by some choice of contact forces within the friction cones. Write down the six equations of force balance for the two bodies in terms of the gravitational forces and the contact forces, and express the conditions that must be satisfied for this assembly to stay standing. How many equations and unknowns are there?

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1qIN975E8alZjwFThLqhwv0pbtmIGsQK5" alt="fig_19.png">
</p>

## Slipping/tipping Question

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1tvKXimvvMZDzNCiQsI55ptb4Soa8xmPd" alt="fig_20.png">
</p>

Source:https://books.google.com.hk/books?id=Xpgi5gSuBxsC&pg=PA657&lpg=PA657&dq=slipping+or+tipping+problems+using+wrenches&source=bl&ots=lVolW9m97Q&sig=ACfU3U03i05F27mvky9ecBo6aak7BNeNMQ&hl=en&sa=X&ved=2ahUKEwjj_9jstMLoAhXkyIsBHVy7BHIQ6AEwAHoECAcQAQ#v=onepage&q=slipping%20or%20tipping%20problems%20using%20wrenches&f=false

Now, consider a box sitting on a table with $$\mu = 1$$ and the friction cone shown below, if the contact mode is SrSr, then in which region lies the center-of-mass? 

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1cetGrZ1Rw5wA990fXQq2jxv4dxgxrN4f" alt="fig_21.png" width="400">
</p>

Using the same principle shown above, we know that the possible regions are A, B, D, and G.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1StK8WkM4v2NYQWvTe0WamsydkZFKods7" alt="fig_22.png">
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
