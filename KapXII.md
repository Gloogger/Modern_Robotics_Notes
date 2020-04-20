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

## Remark on Planar Graphical Method

Because I think the graphical graphical method of wrench is not as intuitive as that of CoR's, so I want to talk about how I understand this method.

I found the definition of the method in another textbook (Springer Handbook of Robotics) more helpful:

> - 27.3.1 Graphical Planar Methods
> Just as homogeneous twist cones for planar problems can be represented as convex signed (+ or −) CoR regions in the plane, homogeneous wrench cones for planar problems can be represented as convex signed regions in the plane. This is called moment labeling [27.14, 34]. Given a collection of lines of force in the plane (e.g., the edges of friction cones from a set of point contacts), the set of all nonnegative linear combinations of these can be represented by labeling all the points in the plane with either a ‘+’ if all resultants make nonnegative moment about that point, a ‘−’ if all make nonpositive moment about that point, a ‘±’ if all make zero moment about that point, and a blank label if there exist resultants making positive moment and resultants making negative moment about that point.

Two steps:
1. When the line of action of an external wrench $$w_{ext}$$ does not pass through any grey areas (signed regions), then this external wrench can be balanced off by the frictional forces provided by the contact points. In other words, the object will remain stationary or quasistatic. 
2. When the line of action of an external wrench $$w_{ext}$$ does pass through any grey areas, draw an opposing wrench of that $$w_{ext}$$, then evaluate whether this opposing wrench is _compatible_ with the existing signs or not.

### Example 1 from Springer Handbook

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1wCIGGohjRuZI7KrTlUSkiJThYCZBHgDj" alt="springer_1.png">
</p>

According to step (1), we can know that the external wrench $$w_{ext}$$ will not cause the object to move because its line of action does not pass through any signed areas. Secondly, according to step (2), if we draw the opposing wrench in the opposite direction of $$w_{ext}$$, we will find that the opposing wrench will still produce a counterclockwise moment for the points inside the grey area marked '+', so this opposing wrench is compatible with the original signed area, thus the object is stationary/quasistatic.

### Example 2 from Springer Handbook

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1AOHKf2eXbFmAR4TPrit-bSkW0LD5IoYE" alt="springer_2.png">
</p>

For figure a), $$w_2$$ does not pass through any gray area, so by step (1) we know that it will not cause the object to move; similarly, looking at the opposing wrench of $$w_2$$ we will find that the opposing wrench will still produce counter-clockwise moments for all points inside the grey area marked '+', then by step (2) we know that it will not cause the object to move. 

The line of action of $$w_1$$ does pass through the gray area, so we must consider its opposing wrench. This opposing wrench separates the signed region into two halfs. For points on the LHS, the opposing wrench still produces a counter-clockweise moment, but for points on the RHS, the opposing wrench will produce negative moments. Therefore, by step (2), $$w_1$$ will cause the object to mvoe.

***

## Implementation of Planar Form Closure Test 
### （by First-Order Analysis）
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

## Implementation of Assembly Stability Test 
### （by First-Order Analysis）
The following implementation is done in MATLAB.

```Matlab
%% This script performs the evaluation of the stability of an assembly 
%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%
%  The scripts takes two input, the first input is a description of the 
%  static mass properties, massList, in the form of
%
%  massList = [    body_ID_1,    mass_1,    x_cm_1,    y_cm_1;
%                  body_ID_2,    mass_2,    x_cm_2,    y_cm_2;
%                    ...          ...         ...        ...  ];
%
%  the second input is a description of the contacts, contactList, in the 
%  form of
%
%  contactList = [   body_ID_1,   body_ID_2,   x_c,   y_c,   theta,   mu;
%                    body_ID_1,   body_ID_4,   x_c,   y_c,   theta,   mu;
%                       ...          ...       ...    ...     ...    ... ];
%
% in which body_ID_1 and body_ID_2 are the ID of bodies:
% body_ID = 0 for ground; body_ID = 1 for body 1; body_ID = 2 for body 2,
% etc. x_c and y_c are the point of contact, theta is the direction of
% contract normal relative to the first body, mu is the coefficient of
% friction at point of contact.
%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020/03/31
%  For personal interests
%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%% Testing
%% Example 1
%  This example was taken from fig.12.37 of the textbook, in which the 
%  structure is not stable. The expected value of IsStable is FALSE.
massList1 = [1, 2, 25, 35;
             2, 5, 66, 42];
contactList1 =  [1, 0,  0,  0, pi/2, 0.1;
                 1, 2, 60, 60,   pi, 0.5;
                 2, 0, 72,  0, pi/2, 0.5;
                 2, 0, 60,  0, pi/2, 0.5];
fprintf('*********Example 1*********\n')
IsStable1 = evaluateStability(massList1,contactList1);
fprintf('*********End of Example 1*********\n')

%% Example 2 (also from fig.12.37 of the textbook)
%  This example is stable as the mu has increased from 0.1 to 0.5 for the
%  first contact. The expected value of IsStable is TRUE.
massList2 = [1, 2,  25, 35;
             2, 10, 66, 42];
contactList2 =  [1, 0,  0,  0, pi/2,  0.5;
                 1, 2, 60, 60,   pi,  0.5;
                 2, 0, 72,  0, pi/2,  0.5;
                 2, 0, 60,  0, pi/2,  0.5];
fprintf('*********Example 2*********\n')
IsStable2 = evaluateStability(massList2,contactList2);
fprintf('*********End of Example 2*********\n')

%% Example 3 (three-body arch, fig.12.27 in textbook)
%  assume mass = 5 for all three bodies
%  The first frictional coefficient that allows the assembly to remain
%  stable is mu = 0.428. Any value below that will result in instability.
massList3 =    [1, 5, 110,  70;
                2, 5, 260, 130;
                3, 5, 410,  70];
mu3 = 0.428;
contactList3 = [1, 0,   0,   0,     pi/2, mu3;
                1, 0,  80,   0,     pi/2, mu3;
                1, 2, 160, 160, -2.81993, mu3;
                1, 2, 180, 100, -2.81993, mu3;
                3, 2, 360, 160, -0.32166, mu3;
                3, 2, 340, 100, -0.32166, mu3;
                3, 0, 440,   0,     pi/2, mu3;
                3, 0, 520,   0,     pi/2, mu3];
fprintf('*********Example 3*********\n')
IsStable3 = evaluateStability(massList3,contactList3);
fprintf('*********End of Example 3*********\n')



%% The evaluation function is defined below
function IsStable = evaluateStability(massList, contactList)
    numBody = size(massList,1);
    numContact = size(contactList,1);
    
    % For each body, the wrenches have 3 dimension, namely [m fx fy],
    % therefore we have 3*numBody columns, and each contact is specified by
    % a friction cone consists of two wrench, therefore we have
    % 2*numContact rows
    F = zeros(3 * numBody, 2 * numContact);
    
    % loop through all contacts
    for i = 1 : numContact
        body_ID_1 = contactList(i,1);
        body_ID_2 = contactList(i,2);
        
        % contact point is augmented to spatial vector because MATLAB 
        % built-in function can only calculate the cross product between 
        % spatial vectors
        contactPoint = [contactList(i,3); ...
                        contactList(i,4); ...
                                0        ];
        
        whichContact = i;           % contact index
        theta = contactList(i,5);
        mu = contactList(i,6);
        
        % if Body 1 is body-on-ground
        if body_ID_1 ~= 0
            angle1 = theta - atan2(mu,1);  % direction of wrench 1 of the 
                                           % friction cone
            angle2 = theta + atan2(mu,1);  % direction of wrench 2 of the 
                                           % friction cone
                                           
            % convert angle into unit vector
            n1 = [cos(angle1); ...
                  sin(angle1); ...
                       0      ];
            n2 = [cos(angle2); ...
                  sin(angle2); ...
                       0      ];
            
            skew_p_n1 = cross(contactPoint, n1);
            skew_p_n2 = cross(contactPoint, n2);
            
            F1 = [skew_p_n1(3); ...
                    n1(1:2,:)  ];
            F2 = [skew_p_n2(3); ...
                    n2(1:2,:)  ];
            
            F( (body_ID_1 - 1) * 3 + 1 : (body_ID_1 * 3), ...
               (whichContact - 1) * 2 + 1) = F1;
            F( (body_ID_1 - 1) * 3 + 1 : (body_ID_1 * 3), ...
               (whichContact - 1) * 2 + 2) = F2;
        end
        
        % if Body 2 is body-on-ground
        if body_ID_2 ~= 0
            angle1 = theta - atan2(mu,1) + pi;  % direction of wrench 1 of 
                                                % the friction cone, + pi
                                                % is used to account for
                                                % 'equal but opposite'
            angle2 = theta + atan2(mu,1) + pi;  % direction of wrench 2 of 
                                                % the friction cone, + pi
                                                % is used to account for
                                                % 'equal but opposite'
                                           
            % convert angle into unit vector
            n1 = [cos(angle1); ...
                  sin(angle1); ...
                       0      ];
            n2 = [cos(angle2); ...
                  sin(angle2); ...
                       0      ];
            
            skew_p_n1 = cross(contactPoint, n1);
            skew_p_n2 = cross(contactPoint, n2);
            
            F1 = [skew_p_n1(3); ...
                    n1(1:2,:)  ];
            F2 = [skew_p_n2(3); ...
                    n2(1:2,:)  ];
            
            F( (body_ID_2 - 1) * 3 + 1 : (body_ID_2 * 3), ...
               (whichContact - 1) * 2 + 1) = F1;
            F( (body_ID_2 - 1) * 3 + 1 : (body_ID_2 * 3), ...
               (whichContact - 1) * 2 + 2) = F2;
        end
    end
    
    % loop through Fext (wrench due to gravity)
    Fext = [];
    g = 9.81;
    theta_g = -pi/2;
    for ii = 1 : numBody
        CM = [massList(ii,3); ...
              massList(ii,4); ...
                    0        ];
        m = massList(ii,2);
        n_g = m .* g .* [cos(theta_g); ...
                         sin(theta_g); ...
                               0      ];
        skew_p_n_g = cross(CM,n_g);
        Fext = [Fext; ... 
                [skew_p_n_g(3);...
                    n_g(1:2,:) ]];
    end
    
    % linear planning coefficient
    f = ones(1,2 * numContact);
    A = -1 .* eye(2 * numContact);
    b = -1 .* ones(1, 2 * numContact);
    Aeq = F;
    beq = -1 .* Fext;
    k = linprog(f,A,b,Aeq,beq);
    
    % preparing output
    fprintf('==================================================\n')
    if isempty(k)
        fprintf('The assembly IS NOT stable.\n')
        IsStable = false;
        fprintf('There does not exist a k such that Fk = 0 while ki >= 1.\n')
    else
        fprintf('The assembly IS stable.\n')
        IsStable = true;
        fprintf('The positve coefficient k in Fk = 0 is:\n')
        disp(k)
    end
    fprintf('==================================================\n')
end
```

### Testing examples

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=19PYn2d6F1PqQ48lME3jESIB9A3Ao8Z_2" alt="Ex1.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1-DMZaiU7FRMT2xkHUuvbSNg_XSQCXVpj" alt="Ex2.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=12OXdZO9eVLRA0degsdS-ZYtGQefG9cAa" alt="Ex3.png">
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

> _**Bonus Exercise**_ Which of the combination of two fingers yields force closure? 

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1dQFph52dcUv6kEZ0Q-LvyEQkv4dj53Y_" alt="fig_24.png">
</p>

***

## Slipping/tipping Question

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1tvKXimvvMZDzNCiQsI55ptb4Soa8xmPd" alt="fig_20.png">
</p>

Source: [Springer Handbook of Robotics](https://books.google.com.hk/books?id=Xpgi5gSuBxsC&pg=PA657&lpg=PA657&dq=slipping+or+tipping+problems+using+wrenches&source=bl&ots=lVolW9m97Q&sig=ACfU3U03i05F27mvky9ecBo6aak7BNeNMQ&hl=en&sa=X&ved=2ahUKEwjj_9jstMLoAhXkyIsBHVy7BHIQ6AEwAHoECAcQAQ#v=onepage&q=slipping%20or%20tipping%20problems%20using%20wrenches&f=false)

### Example 1
Now, consider a box sitting on a table with $$\mu = 1$$ and the friction cone shown below, if the contact mode is SrSr, then in which region lies the center-of-mass? 

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1cetGrZ1Rw5wA990fXQq2jxv4dxgxrN4f" alt="fig_21.png" width="400">
</p>

Using the same principle shown above, we know that the possible regions that contain the center-of-mass in SrSr contact mode are A, B, D, and G.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1StK8WkM4v2NYQWvTe0WamsydkZFKods7" alt="fig_22.png" width="500">
</p>

For BR mode, the regions are A, C, D, E, F, G.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1oZ4wd7fUQYujP7wLzi3525WmbbZb1tNN" alt="fig_23.png" width="500">
</p>


***

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
