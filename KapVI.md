---
layout: post
mathjax: true
title: 'Kapitel VI: Inverse Kinematics'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapV.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapVII.html';">Next</button></span>
</p>

## Useful Notes and Equations
For a comprehensive and thorough summary of the theory, check MuChenSun's wonderful note [here](https://muchensun.github.io/ModernRoboticsCourseNotes/ch6.html).

My own notes:
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1z2S-qF4UOxz2AnvIoYwMR71L28oSa0O1" alt="note_1.png">
</p>


### Useful Equations:

***

## Textbook Exercises Attempts
> _**Exercise 6.8**_ Use Newton–Raphson iterative numerical root finding to perform two steps in finding the root of
>
  $$ g(x,y) = \begin{bmatrix}
    x^2 - 4\\
    y^2 - 9\\
  \end{bmatrix}
  $$
>  
>  when your initial guess is $$(x^{0}, y^{0})=(1,1)$$. Write the general form of the gradient (for any guess $$(x,y)$$) and compute the results of the first two iterations. You can do this by hand or write a program. Also, give all the correct roots, not just the one that would be found from your initial guess. How many are there?
  
From $$g(x,y)$$ we know that $$x_{d} = (4,9)^{T}$$, $$f(\theta)=(x^{2}, y^{2})^{T}$$.

$$ \frac{\delta f}{\delta \theta} = J(\theta) = \begin{bmatrix} 
    2x & 0 \\
    0 & 2y \\
\end{bmatrix}
$$

$$ J^{-1}(\theta) = \begin{bmatrix}
    \frac{1}{2x} & 0 \\
    0 & \frac{1}{2y} \\
\end{bmatrix}
$$

$$
\begin{align}
    \begin{split}
        \theta^{1} &= \begin{bmatrix}
            \frac{1}{2} & 0 \\
            0 & \frac{1}{2} \\
        \end{bmatrix} \begin{bmatrix}
            4-1^{2} \\
            9-1^{2} \\
        \end{bmatrix} + \begin{bmatrix}
            1 \\
            1 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            2.5 \\
            5
        \end{bmatrix} \\
        \theta_{2} &= \begin{bmatrix}
            \frac{1}{5} & 0 \\
            0 & \frac{1}{10} \\
        \end{bmatrix} \begin{bmatrix}
            4 - 2.5^{2} \\
            9 - 5^{2} \\
        \end{bmatrix} + \begin{bmatrix}
            2.5 \\
            5 \\
        \end{bmatrix} \\
        &= \begin{bmatrix}
            2.05 \\
            3.4 \\
        \end{bmatrix}
    \end{split}
\end{align}
$$

There should be a total of 4 solutions, since x could be -2 or 2, and y could be -3 or 3.


***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
