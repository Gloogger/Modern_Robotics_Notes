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

***

The rest of the chapter is basically the same old string-damper system(which is familiar to any mechanical engineering student), therefore these exercises are skipped here.

***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
