---
layout: post
mathjax: true
title: 'Kapitel X: Motion Planning'
description: Useful Equations and Textbook Exercise Attempts
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapIX.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='KapXI.html';">Next</button></span>
</p>

## Useful Notes and Equations
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1mnDxiqs1IdvNmHewFYOWmvMyqrbTcA24" alt="note_1.png">
</p>


### Useful Equations:

***

## Textbook Exercises Attempts

_**Exercise 10.9**_ Implement an A * path planner for a point robot in a plane with obstacles. The planar region is a $$100\times 100$$ area. The program generates a graph consisting of _N_ nodes and _E_ edges, where _N_ and _E_ are chosen by the user. After generating _N_ randomly chosen nodes, the program should connect randomly chosen nodes by edges until _E_ unique edges have been generated. The cost associated with each edge is the Euclidean distance between the nodes. Finally, the program should display the graph, search the graph using A * for the shortest path between nodes 1 and N, and display the shortest path or indicate FAILURE if no path exists. The heuristic cost-to-go is the Euclidean distance to the goal.

The following implementation is done in MATLAB:

```Matlab
%% This script takes two input files, namely the nodes.csv in the layout of
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  |       Column 4       |  
%  |  node ID   |     x      |     y      | heuristic-cost-to-go |
%  |    ....    |    ...     |    ...     |         ...          |
%  and the edges.csv in the layout of
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  |  
%  |  node ID1  |  node ID2  |  edge_cost |
%  |    ....    |    ...     |    ...     | 
%  N.B. that the edge between node 4 and 7 is deleted as per Assignment
%  requires.
%  This script calculates the collision-free path using the A * algorithm, 
%  the output is saved as the 'path.csv'
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020/03/04
%  For the purpose of finishing textbook exercises.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%--------------------------------------------------------------------------
%                      Start of the Main Script
%--------------------------------------------------------------------------

%% Read .csv Input Files
nodes = readmatrix('nodes.csv','NumHeaderLines',8); % Skip the first 8 rows
                                                    % of comments
edges = readmatrix('edges.csv','NumHeaderLines',6); % Skip the first 6 rows
                                                    % of comments
%% A star Algorithm
N = length(nodes);      % get total No. of nodes
goalSet = N;            % Assuming the last node is the goal node

% Initialize Data Structures
OPEN = [1];
CLOSED = [];

% Create cost matrix
cost = createCostMatrix(edges,N); % cost(i,j) will return the cost of
                                      % going from node i to node j

% Creaet minimal cost array
myInf = max(edges(:,3)) * 100000;     % A really big number
past_cost = [0 myInf.*ones(1,N-1)];   % 0 cost to node 1, unknown for rest

parent = zeros(1,N);            % A list storing the parent of the current 

while ~isempty(OPEN)
    current = OPEN(1);
    OPEN(1) = [];
    CLOSED = [CLOSED current];
    
    if current == goalSet 
        fprintf('Path found! Done!\n')
        break
    end
    
    nbr = getNeighbour(cost,current); % get the neighbours of current node
    
    for i = 1:length(nbr)
        if ~max(CLOSED==nbr(i))                     % if nbr not in CLOSED
            tentative_past_cost = past_cost(current) + cost(current,nbr(i));
            if tentative_past_cost < past_cost(nbr(i))
                past_cost(nbr(i)) = tentative_past_cost;
                parent(nbr(i)) = current;  % a 'pseudo-' binary search tree
                OPEN = [OPEN nbr(i)];      % push neighbour nodes into OPEN
            end
        end
    end
    OPEN = sortOPEN(OPEN,past_cost,nodes); % sort OPEN according to costs
end    

optPath = getOptPath(parent);      % Get the optimal path
fprintf('The optimal path found is:\n')
disp(optPath)
writematrix(optPath,'path.csv','Delimiter','comma')

%--------------------------------------------------------------------------
%                       End of the Main Script
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%--------------------------------------------------------------------------
%                       Functions are defined below
%--------------------------------------------------------------------------

%% Create Cost Matrix
%  This function takes edges and number of nodes as inputs, and it returns
%  a cost matrix that takes 
%
function cost = createCostMatrix(edges,N)
    numNode = length(edges);
    cost = -1 .* ones(N);
    for i = 1:numNode
        node1 = edges(i,1);
        node2 = edges(i,2);
        edge_weight = edges(i,3);
        cost(node1,node2) = edge_weight;
        cost(node2,node1) = edge_weight;
    end

end

%% Get neighbour node of current node
function nbr = getNeighbour(cost,current)
    numNode = length(cost);
    tempNBR = [];
    for ii = 1:numNode
        if cost(current,ii) > 0
            tempNBR = [tempNBR ii];
        end
    end
    nbr = tempNBR;
end

%% Sort OPEN according to est_total_cost = past_cost(nbr) + ...
%  heuristic_cost_to_go(nbr)
function sortedOPEN = sortOPEN(OPEN,past_cost,nodes)
    heuristic_cost_to_go = nodes(:,4);
    N = length(OPEN);
    cost_open = [OPEN' ones(N,1)]; 
    for i = 1:N
        cost_open(i,2) = past_cost(cost_open(i,1)) + ...
                         heuristic_cost_to_go(cost_open(i));
    end
    A = sortrows(cost_open,2);
    sortedOPEN = A(:,1)';
end

%% Get optimal path from parent
function optPath = getOptPath(parent)
    N = length(parent);
    optPath = [N];
    i = N;
    while 1
        if i == 1
            break
        end
        optPath = [parent(i) optPath];
        i = parent(i);
    end
end

%--------------------------------------------------------------------------
%                       End of Function Definition
%                   This is the very end of this submission
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```

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
