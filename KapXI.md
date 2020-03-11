---
layout: post
mathjax: true
title: 'Kapitel XI: Robot Control'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapX.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapXII.html';">Next</button></span>
</p>

## Useful Notes and Equations

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1GFg44TORbXqWhLeVb-plRI6V1l8LPiYK" alt="note_1.png">
</p>


### Useful Equations:

***

## Textbook Exercises Attempts

> _**Exercise**_ 11.2 The $$2\%$$ settling time of an underdamped second-order system is approximately $$t = \frac{4}{\zeta \omega_{n}}$$, for $$e^{-\zeta \omega_{n} t}=0.02$$. What is the $$5\%$$ settling time?

By definition, the percentage settling time is defined as $$N\%t = -\frac{\ln\left(N/100 \right)}{\zeta \omega_{n}}$$. Therefore, $$5\%t = -\frac{\ln\left(5/100 \right)}{t} \approx \frac{3}{\zeta \omega_{n}} $$.

> _**Exercise 11.3**_ Solve for any constants and give the specific equation for an underdamped second-order system with $$\omega_n = 4$$, $$\zeta = 0.2$$, $$\theta_{e}(0) = 1$$, and $$\dot{\theta_{e}}(0) = 0$$. Calculate the damped natural frequency, approximate overshoot, and $$2\%$$ settling time. Plot the solution on a computer and measure the exact overshoot and settling time.

$$
\begin{align}
    \begin{split}
        \omega_d &= \omega_n \sqrt{1-\zeta^2}\\
        &= 4 \sqrt{1-0.2^2} \\
        &= 3.9192 \\
    \end{split}
\end{align}
$$

$$
\begin{align}
    \begin{split}
        \text{overshoot} &= \text{exp}\left( \frac{-\pi \zeta}{\sqrt{1-\zeta^2}} \right) \\
        &= 0.5266 \\
        &= 52.66\% \\
    \end{split}
\end{align}
$$

$$
\begin{align}
    \begin{split}
        2\%t &= -\frac{\text{ln}(0.02)}{\zeta \omega_n} \\
        &= 4.8900\,\text{rad/s} \\
    \end{split}
\end{align}
$$


<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1rAZ-0cepe9dzwQ-LPIOiA0kHv6262Vru" alt="fig_1.png" width="500">
</p>

> _**Exercise 11.4**_ Solve for any constants and give the specific equation for an underdamped second-order system with $$\omega_n = 10$$, $$\zeta = 0.1$$, $$\theta_e (0) = 0$$, and $$\dot{\theta_e} (0) = 1$$. Calculate the damped natural frequency. Plot the solution on a computer.

$$\omega_d = \omega_n \sqrt{1-\zeta^2} = 9.94987\,\text{rad/s}$$

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1E4cNcTbYAii87Rd3e0WwtIcQr9Yi0PL4" alt="fig_2.png" width="500">
</p>

> _**Exercise 11.5**_ Consider a pendulum in a gravitational field with $$g = 10 \;\text{m/s}^2$$. The pendulum consists of a $$2$$ kg mass at the end of a $$1$$ m massless rod. The pendulum joint has a viscous-friction coefficient of $$b = 0.1\,\text{Nms/rad}$$.
> - (a) Write the equation of motion of the pendulum in terms of $$\theta$$, where $$\theta=0$$ corresponds to the “hanging down” configuration.
> - (b) Linearize the equation of motion about the stable “hanging down” equilibrium. To do this, replace any trigonometric terms in $$\theta$$ with the linear term in the Taylor expansion. Give the effective mass and spring constants $$m$$ and $$k$$ in the linearized dynamics $$m \ddot{\theta} + b\dot{\theta}+k\theta =0$$. At the stable equilibrium, what is the damping ratio? Is the system underdamped, critically damped, or overdamped? If it is underdamped, what is the damped natural frequency? What is the time constant of convergence to the equi- librium and the $$2\%$$ settling time?
> - (c) Now write the linearized equations of motion for $$\theta=0$$ at the balanced upright configuration. What is the e↵ective spring constant $$k$$?
> - (d) You add a motor at the joint of the pendulum to stabilize the upright position, and you choose a P controller $$\tau = K_p \theta$$. For what values of $$K_p$$ is the upright position stable?

- (a) $$\ddot{\theta} + 10 \text{sin}(\theta) = 0$$
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1-2u56jTaOty0Vg_W9QSsdjyZEMiuYcyw" alt="fig_3.png" width="600">
</p>

- (b) $$1\cdot \ddot{\theta} + 0 \cdot \dot{\theta} + 10\cdot \theta = 0$$
  By observation, we get $$m=1$$, $$b = 0$$, $$k=10$$. 
  The natural frequency is $$\omega_n = \sqrt{\frac{k}{m}} = \sqrt{\frac{10}{1}} = \sqrt{10}$$. 
  From $$2\zeta \omega_n = 0$$, the damping ratio is obtained as $$\zeta = 0$$. Since $$\zeta < 0$$, the system is **underdamped**. 
  The damped natural frequency is $$\omega_d = \omega_n \sqrt{1-\zeta^2} = \sqrt{10}$$. 
  The $$2\%$$ settling time is $$2\%t = -\frac{ln(0.02)}{\zeta \omega_n} \approx \frac{4}{0} = \infty$$.

- (c) Don't know how to do.
- (d) Don't know how to do.

***

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
