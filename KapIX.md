---
layout: post
mathjax: true
title: 'Kapitel IX: Trajectory Generation'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapVIII.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapX.html';">Next</button></span>
</p>

## Useful Notes and Equations

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1n1aBoej-dckoSiwXJ36s1Jn7wM62V-HK" alt="note_1.png">
</p>


### Useful Equations:

***

## Textbook Exercises Attempts
> _**Exercise 9.1**_ Consider an elliptical path in the $$(x,y)$$-plane. The path starts at $$(0,0)$$ and proceeds clockwise to $$(2,1)$$, $$(4,0)$$, $$(2,-1)$$, and back to $$(0,0)$$ (Figure 9.15). Write the path as a function of $$s \in [0, 1]$$.
    <p align="center">
        <img src="https://drive.google.com/uc?export=view&id=1Qy4lC75kuGIOE9nMml4BELj8fJYLEcwW" alt="fig_1.png" width="400">
    </p>

List the table below:

| $$s$$  | $$x$$ | $$y$$ |
| ------- | ------ | ------------- |
| 0 | 0  | 0 |
| $$\frac{1}{4}$$ | $$2$$ | $$1$$ |
| $$\frac{2}{4}$$ | $$4$$ | $$0$$ |
| $$\frac{3}{4}$$ | $$2$$ | $$-1$$ |
| $$\frac{4}{4}$$ | $$0$$ | $$0$$ |

By observation, we get,

$$
\begin{align}
    \begin{split}
        x &= 2\left(1-\cos (2\pi s) \right) \\
        y &= \sin (2\pi s) \\
    \end{split}
\end{align}
$$

> _**Exercise 9.2**_ A cylindrical path in $$X = (x, y, z)$$ is given by $$x = \cos(2\pi s)$$, $$y = \sin(2\pi s)$$, $$z = 2s$$, $$s \in [0,1]$$, and its time scaling is $$s(t) = \frac{1}{4} t + \frac{1}{8} t^{2}$$, $$t \in [0, 2]$$. Write done $$\dot{X}$$ and $$\ddot{X}$$.

$$
\begin{align}
    \begin{split}
        \dot{X} &= \frac{dX}{ds} \frac{ds}{dt} \\
        &= \begin{bmatrix}
            -2 \pi \sin (2\pi s) \\
            2 \pi \cos (2\pi s) \\
            2 \\
        \end{bmatrix} \cdot \left( \frac{1}{4} + \frac{1}{4} t \right)
    \end{split}
\end{align}
$$

Similarly,

$$
\begin{align}
    \begin{split}
        \ddot{X} &= \frac{d^{2}X}{ds^{s}} \frac{ds}{dt} + \frac{dX}{ds} \frac{d^{2}s}{dt^{2}} \\
        &= \ldots \\
    \end{split}
\end{align}
$$

> _**Exercise 9.3**_  Blah Blah

Don't know how to do.

> _**Exercise 9.4**_ Consider a straight-line path $$\theta (s) = \theta_{\text{start}} + s (\theta_{\text{end}} - \theta_{\text{start}})$$, $$s \in [0,1]$$ from $$\theta_{\text{start}} = (0, 0)$$ to $$\theta_{\text{end}} = (\pi, \pi/3)$$. The motion starts and ends at rest. The feasible joint velocities are $$\lvert \dot{\theta_{1}} \rvert$$, $$\lvert \dot{\theta_{2}}\rvert \leq 2 \, \text{rad/s} $$ and the feasible joint accelerations are $$\lvert \ddot{\theta_{1}} \rvert$$, $$\ddot{\theta_{2}} \leq 0.5 \, \text{rad/s}^{2}$$. Find the fastest motion time $$T$$ using a cubic time scaling that satisfies the joint velocity and acceleration limits.

Because the two joints have the same velocity and acceleration limits, we just need to consider the largest one, which in this case is $$\theta_{\text{end,}\,1} = \pi$$. Then, $$\theta(s) = 0 + s (\pi - 0) =  \pi s$$. The terminal constraints for this cubic time scaling function are:

$$
\begin{cases}
  s(0) = 0, & \text{ipso facto} \\
  s(T) = 1, & \text{ipso facto} \\
  \dot{s}(0) = 0, & \text{starts at rest} \\
  \dot{s}(T) = 0, & \text{ends at rest} \\
\end{cases}
$$

Substitute the above constraints into the cubic time scaling $$s(t) = a_{0} + a_{1}t + a_{2} t^{2} + a_{3} t^{3}$$, solve

$$
\left( \begin{array}{cccc|c} 
1 & T & T^2 & T^3 & 1 \\ 
1 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 \\
0 & 1 & 2T & 3T^2 & 0 \\
\end{array} \right)
$$

Thus, the coefficents are obtained as $$\left\{ a_{0}=0,\, a_{1}=0,\, a_{2} = \frac{3}{T^{2}},\, a_{3}=- \frac{2}{T^{3}} \right\}$$. Substitute the coefficients into the path function gives

$$
\begin{align}
    \begin{split}
        \theta &= \pi s = \frac{3\pi}{T^{2}} t^{2} - \frac{2\pi}{T^{3}} t^{3} \\
        \dot{\theta} &= \pi \dot{s} = \frac{6\pi}{T^{2}} t - \frac{6\pi}{T^{3}} t^{2} \\
        \ddot{\theta} &= \pi \ddot{s} = \frac{6\pi}{T^{2}} - \frac{12\pi}{T^{3}} t \\
    \end{split}
\end{align}
$$

Now we should find the minimal motion time $$T$$ in accordance with the velocity and acceleration limits. Because we don't know which one is controlling, so we have to calculate both. 

The elocity limit is $$ \lvert \dot{\theta_{1}} \rvert \leq 2 $$. Since we know the maximal speed occurs at $$t=\frac{T}{2}$$, we have,

$$
\begin{align}
    \begin{split}
        \dot{\theta}_{max} &= \dot{\theta}\left(\frac{T}{2}\right) \\
        &= \frac{6\pi}{T^{2}} \frac{T}{2} - \frac{6\pi}{T^{3}} \left(\frac{T}{2}\right)^{2} \\
        &= \frac{3\pi}{2T} \leq 2 \\
        T_{\text{min, vel}} &= \frac{3\pi}{4} \approx 2.3562 
    \end{split}
\end{align}
$$

The cceleration limit is $$ \lvert \ddot{\theta_{1}} \rvert \leq \frac{1}{2}$$. Since we know the maximal acceleration occurs at either terminals, i.e., $$t=0$$ or $$t=T$$, we have,

$$
\begin{align}
    \begin{split}
        \ddot{\theta}_{max} &= \ddot{\theta}\left(0\right) \\
        &= \frac{6\pi}{T^{2}} \leq \frac{1}{2} \\
        T_{\text{min, acc}} &= 2 \sqrt{3\pi} \approx 6.1400 
    \end{split}
\end{align}
$$

Thus, the minimal motion time is, $$T=\max( T_{\text{min, vel}},\, T_{\text{min, acc}})=2 \sqrt{3\pi}$$.

> _**Exercise 9.5**_ Find the fifth-order polynomial time scaling that satisfies $$s(T) =1$$ and $$s(0) = \dot{s}(0) = \ddot{s}(0) = \dot{s}(T) = \ddot{s}(T) = 0$$.

Let the fifth-order time scaling be $$s(t) = a_{0} + a_{1} t + a_{2} t^{2} + a_{3} t^{3} + a_{4} t^{4} + a_{5} t^{5}$$. Solve

$$\left( \begin{array}{cccccc|c} 
1 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 \\
1 & T & T^2 & T^3 & T^4 & T^5 & 1 \\
0 & 1 & 2T & 3T^2 & 4T^3 & 5T^4 & 0 \\
0 & 0 & 1 & 6T & 12T^2 & 20T^3 & 0 \\ 
\end{array} \right)
$$

The solution is $$\left\{ a_{0}=0,\, a_{1}=0,\, a_{2}=0,\, a_{3}=\frac{10}{T^3},\, a_{4}=-\frac{15}{T^4},\, a_{5}=\frac{6}{T^5} \right\}$$.

> _**Exercise 9.7**_ If you want to use a polynomial time scaling for point-to-point motion with zero initial and final velocities, accelerations, and jerks, what would be the minimum order of the polynomial?

7.

> _**Exercise 9.8**_ Prove that the trapezoidal time scaling, using the maximum allowable acceleration a and velocity v, minimizes the time of motion T.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1uAwOr6Lt9uy10qD4wGt3jhlaH1FLz_gg" alt="fig_2.png" width="550">
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
