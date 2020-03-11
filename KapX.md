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

## Implementation of A * Search Algorithm

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
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```

***

## Implementation of RRT (Rapid Random Trees)

The following implementation is done in MATLAB:

```Matlab
%% This script takes one input file, namely the obstacles.csv containing
%  only circular objects, specified using its center coordinates (x,y) and
%  the radius, in the layout of:
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  | 
%  |     x      |     y      |   radius   |
%  |    ....    |    ...     |    ...     |
%  The script will output 3 files, namely the nodes.csv, edges.csv, and the
%  path.csv in the prescribed layout, except without the comment
%  headerlines.
%  *****************This script adopted the RRT method*********************
%  2D C-space is (x,y) in [-0.5,0.5] x [-0.5,0.5]
%  Start point: (-0.5,-0.5)
%  Goal point:  (0.5,0.5)
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020/03/05

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%--------------------------------------------------------------------------
%                      Start of the Main Script
%--------------------------------------------------------------------------

%% Initialization
clear;
clc;

%% Define Input Variables
% Start and goal nodes
startNode = [-0.5 -0.5];
goalNode = [0.5 0.5];

% C-space size
% q = [q_min q_max]
q1 = [startNode(1) goalNode(1)];
q2 = [startNode(2) goalNode(2)];

% Read obstacles.csv file
obstacles = readmatrix('obstacles.csv','NumHeaderLines',5); % skip first 5
                                                            % lines of 
                                                            % comments

% C-space resolution
resC = 500;     % Divide resC intervals between q_min and q_max

% Define how long the node list should be
maxCount = resC;      % doesn't have to resC, any magic number is fine
maxIteration = 100;   % maximal amount of iteration

%% RRT Algorithm
nodeList = cell(1,maxIteration);    % stores the nodes from each iteration
edgeList = cell(1,maxIteration);    % stores the edges from each iteration

for aa = 1:maxIteration
    %% Initialize nodes in the layout of
    %  |  Column 1  |  Column 2  |  Column 3  |       Column 4       |  
    %  |  node ID   |     x      |     y      | heuristic-cost-to-go |
    %  |    ....    |    ...     |    ...     |         ...          |
    nodes = [1 startNode EuclideanCost(startNode,goalNode)];

    %% Initialize edges in the layout of
    %  |  Column 1  |  Column 2  |  Column 3  |  
    %  |  node ID1  |  node ID2  |  edge_cost |
    %  |    ....    |    ...     |    ...     |
    edges = [];
    
    i = 1;
    %ii = 1;
    n = 0;
    while i < maxCount
        % Define biasLvl
        biasLvl = 0.9;
        if rand >= biasLvl
            biasOn = 1;
        else
            biasOn = 0;
        end
        % Get random sample nodes from X
        XSampSpace = getRandomNodes(q1,q2,resC,biasOn);
        x_samp = XSampSpace(i,:);

        near_ID = getNearNode(x_samp,nodes);
        x_near = [nodes(near_ID,2) nodes(near_ID,3)];

        IsFree = isCollisionFree(x_samp,x_near,obstacles);
        if IsFree
            % Include x_samp into nodes
            ii = size(nodes,1);
            heuristic_cost_to_go = EuclideanCost(x_samp,goalNode);
            nodes = [nodes;...
                     ii+1 x_samp heuristic_cost_to_go];
            % Include edge between x_samp and x_near
            edge_cost = EuclideanCost(x_samp,x_near);
            edges = [edges;...
                     near_ID ii+1 edge_cost];        
            % Success Exit
            if isequal(x_samp, goalNode)
                break;
            end  
        end
        %}
        i = i + 1;
    end
    nodeList{aa} = nodes;
    edgeList{aa} = edges;
end

%% Find closest end point
lastNodeCost = [[1:maxIteration]' zeros(maxIteration,1)];
for bb = 1:maxIteration
    lastNode = [nodeList{bb}(end,2) nodeList{bb}(end,3)];
    lastNodeCost(bb,2) = EuclideanCost(lastNode,goalNode);
end
lastNodeCost = sortrows(lastNodeCost,2);
bestIndex = lastNodeCost(1,1);
bestNodes = nodeList{bestIndex};
bestEdges = edgeList{bestIndex};

%% Generate optimal path based on nodes and edges using A * Search
path = A_Star_Search(bestNodes,bestEdges);

%% Preparing Output .csv Files
writematrix(bestNodes,'nodes.csv','Delimiter','comma')
writematrix(bestEdges,'edges.csv','Delimiter','comma')
writematrix(path,'path.csv','Delimiter','comma')

%--------------------------------------------------------------------------
%                       End of the Main Script
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%--------------------------------------------------------------------------
%                       Functions are defined below
%--------------------------------------------------------------------------

%% Generate random nodes inside C-space
function XSampSpace = getRandomNodes(q1,q2,resC,biasOn)
    %rng(0,'twister');   % make 'pseodo-random' results repeatable
    q1_min = q1(1);
    q1_max = q1(2);
    q2_min = q2(1);
    q2_max = q2(2);
    
    xx = (q1_max - q1_min) .* rand(resC,1) + q1_min; % generate resC numbers
                                                     % between q1_max &
                                                     % q1_min
    yy = (q2_max - q2_min) .* rand(resC,1) + q2_min; % generate resC numbers
                                                     % between q1_max &
                                                     % q1_min    
    if biasOn
        XSampSpace = [sort(xx) sort(yy)];
    elseif rand > 0.5
        XSampSpace = [xx sort(yy)];
    else
        XSampSpace = [sort(xx) yy];
    end
end

%% Calculate the Euclidean costs between two nodes
function euclid_cost = EuclideanCost(node1,node2)
    euclid_cost = sqrt((node1(1)-node2(1))^2 + (node1(2)-node2(2))^2);
end

%% Get the nearest node in 'nodes' from current x_samp
function near_ID = getNearNode(x_samp,nodes)
    N = size(nodes,1);
    for i = 1:N
        costs(i,1) = i;
        costs(i,2) = EuclideanCost(x_samp, [nodes(i,2) nodes(i,3)]);
    end
    costs = sortrows(costs,2);
    near_ID = costs(1,1);
end

%% Check if the x_samp is 
%  1. inside the obstacle, or
%  2. has its edge with x_samp crossing the boundaries of the obstacle

%  Because we assume the robot to be a particle(i.e., its radius r = 0),
%  the radius of the obstacle R will grow to R+r to balance this assumption
%  However, the actual radius of the robot is not known. Therefore, we
%  define a factor of safety (FS) to enlarge the distance between the robot
%  and the boundary of the obstacle. FS in [0, 1], the larger the FS, the
%  more distant the robot is from the obstacle. 
%  FS = 0 => contact with obsticle's boundary
%  FS = 1 => 1 diameter (of the obsticle) away from the center of the 
%  obsticle

function IsFree = isCollisionFree(x_samp,x_near,obstacles)
    N = size(obstacles,1);
    FS = 0.10;
    IsFree = true;
    for i = 1:N
        center = [obstacles(i,1) obstacles(i,2)];
        diameter = obstacles(i,3);
        %****************check if coincides with the center****************
        if isequal(x_samp,center)
           IsFree = false; 
        end
        %******************check if inside the obstacle********************
        distance = EuclideanCost(x_samp, center);
        if distance <= (diameter / (2-FS) )
            IsFree = false;
        end
        %******************************************************************
        
        %*********check if edge crosses the obstacle's boundary************
        % Checking if the edge is blocked by the obstacle's boundary is the
        % same as checking if a line defined by two points intersects with
        % a circle defined by (center, radius). See this math stackexchange
        % discussion for detail:
        % https://math.stackexchange.com/questions/2837/how-to-tell-if-a-li
        % ne-segment-intersects-with-a-circle/2844#2844
        line = x_samp - x_near;
        normLine = line / norm(line);
        node2center = center - x_near;
        normal_distance = abs(node2center(1)*normLine(2) - ...
                              node2center(2)*normLine(1));
        if normal_distance <= (diameter / 2)
            PQ = EuclideanCost(x_samp,x_near);
            QC = EuclideanCost(x_samp,center);
            PC = EuclideanCost(x_near,center);
            cosPQC = (PC^2 - PQ^2 - QC^2) / (-2 * PQ * QC);
            cosQPC = (QC^2 - PQ^2 - PC^2) / (-2 * PQ * PC);
            if (cosPQC >= 0) && (cosQPC >= 0)
                IsFree = false;
            end
        end
        %******************************************************************        
    end
end

%% Gets the optimal path using A * Search, takes nodes and edges in form of
%  nodes
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  |       Column 4       |  
%  |  node ID   |     x      |     y      | heuristic-cost-to-go |
%  |    ....    |    ...     |    ...     |         ...          |
%  edges
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  |  
%  |  node ID1  |  node ID2  |  edge_cost |
%  |    ....    |    ...     |    ...     | 

function optPath = A_Star_Search(nodes,edges)
    N = size(nodes,1);      % get total No. of nodes
    goalSet = N;            % Assuming the last node is the goal node

    % Initialize Data Structures
    OPEN = [1];
    CLOSED = [];

    % Create cost matrix
    cost = createCostMatrix(edges,N); % cost(i,j) will return the cost of
                                      % going from node i to node j

    % Creaet minimal cost array
    myInf = max(edges(:,3)) * 100000;  % A really big number
    past_cost = [0 myInf.*ones(1,N-1)];% 0 cost to node 1, unknown for rest

    parent = zeros(1,N);         % A list storing the parent of the current 
    %% A star Algorithm
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
            if ~max(CLOSED==nbr(i))                  % if nbr not in CLOSED
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
end

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
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

```

### Results
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1lc3fSL48dHMnDkYjPmhwqqi_Xj68euUc" alt="fig_1.png">
</p>

Note that _resC_ is the number of intervals used to discretize the C-space.

***

## Implementation of PRM (Probabilistic RoadMap)

The following implementation is done in MATLAB:

```Matlab
%% This script takes one input file, namely the obstacles.csv containing
%  only circular objects, specified using its center coordinates (x,y) and
%  the radius, in the layout of:
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  | 
%  |     x      |     y      |   radius   |
%  |    ....    |    ...     |    ...     |
%  The script will output 3 files, namely the nodes.csv, edges.csv, and the
%  path.csv in the prescribed layout, except without the comment
%  headerlines.
%  *****************This script adopted the RRT method*********************
%  2D C-space is (x,y) in [-0.5,0.5] x [-0.5,0.5]
%  Start point: (-0.5,-0.5)
%  Goal point:  (0.5,0.5)
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020/03/09

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%--------------------------------------------------------------------------
%                      Start of the Main Script
%--------------------------------------------------------------------------

%% Initialization
clear;
clc;

%% Define Input Variables
% Start and goal nodes
startNode = [-0.5 -0.5];
goalNode = [0.5 0.5];

% Read obstacles.csv file
obstacles = readmatrix('obstacles.csv','NumHeaderLines',5); % skip first 5
                                                            % lines of 
                                                            % comments
%% PRM algorithm
N = 500;         % Create N sample node in C-space
                 % Nodes in C-free will be less than N depends on the total
                 % area of the obstacles
%% Generate Roadmap in the layout of
% Roadmap{1} = Rnodes([ | x | y | ])
% Roadmap{2} = Redges([ | ID1 | ID2 | edge_cost | ])

% Generate Nodes in C-free
Rnodes = getRNodes(N,startNode,goalNode,obstacles);

% Include startNode and goalNode to Rnodes
Rnodes = [startNode; Rnodes; goalNode];

% Add node ID to Rnodes
% Rnodes = [ | ID | x | y | ]
Rnodes = [[1:size(Rnodes,1)]' Rnodes];

% Add heuristic-cost-to-go to Rnodes, make it in the layout of nodes
% Rnodes = [ | ID | x | y | h_ctg | ]
for i = 1:Rnodes(end,1)
    h_ctg = EuclideanCost([Rnodes(i,2) Rnodes(i,3)], goalNode);
    Rnodes(i,4) = h_ctg;
end
nodes = Rnodes;

% Generate collision-free edges between node q and its k closest neighbours
k = 5;  % How many neighbours to be included

node_len = nodes(end,1);
edges = [];
for i = 1:node_len
    current_ID = i;
    current_node = [nodes(i,2) nodes(i,3)];
    near_ID = getNearNode(current_node,nodes,k,obstacles);
    edge_cost = [];
    for ii = 1:k
        edge_cost = [edge_cost; ...
                     EuclideanCost(current_node,...
                     [nodes(near_ID(ii),2) nodes(near_ID(ii),3)])];
    end
    edges = [edges; (current_ID .* ones(k,1)) near_ID edge_cost];
end

%% Generate optimal path based on nodes and edges using A * Search
path = A_Star_Search(nodes,edges);

%% Preparing Output .csv Files
writematrix(nodes,'nodes.csv','Delimiter','comma')
writematrix(edges,'edges.csv','Delimiter','comma')
writematrix(path,'path.csv','Delimiter','comma')

%--------------------------------------------------------------------------
%                       End of the Main Script
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%--------------------------------------------------------------------------
%                       Functions are defined below
%--------------------------------------------------------------------------

%% Generate N nodes in C_free
function RNodes = getRNodes(N,startNode,goalNode,obstacles)
%  Because we assume the robot to be a particle(i.e., its radius r = 0),
%  the radius of the obstacle R will grow to R+r to balance this assumption
%  However, the actual radius of the robot is not known. Therefore, we
%  define a factor of safety (FS) to enlarge the distance between the robot
%  and the boundary of the obstacle. FS in [0, 1], the larger the FS, the
%  more distant the robot is from the obstacle. 
%  FS = 0 => contact with obsticle's boundary
%  FS = 1 => 1 diameter (of the obsticle) away from the center of the 
%  obsticle
    q1_min = startNode(1);
    q1_max = goalNode(1);
    q2_min = startNode(2);
    q2_max = goalNode(2);
    FS = 0.2;
    x_obs = obstacles(:,1);       % x-coordinates of the obstacle's center
    y_obs = obstacles(:,2);       % y-coordinates of the obstacle's center
    r_obs = obstacles(:,3) ./ (2-FS) ; % radii of the obstacles
    %x_obs = 350.5; y_obs = 350.5; r_obs = 50;
    theta = 0 : pi/50 : 2*pi;
    obs_len = size(obstacles,1);
    poly_obs = cell(1,obs_len);
    
    xx = (q1_max - q1_min) .* rand(N,1) + q1_min; % generate resC numbers
                                                     % between q1_max &
                                                     % q1_min
    yy = (q2_max - q2_min) .* rand(N,1) + q2_min; % generate resC numbers
                                                     % between q1_max &
                                                     % q1_min
    Cnode = [xx yy];

    for i = 1:obs_len
        % Create polygons
        poly_x = r_obs(i) .* cos(theta) + x_obs(i);
        poly_y = r_obs(i) .* sin(theta) + y_obs(i);
        poly_obs{i} = polyshape(poly_x(1:end-1),poly_y(1:end-1));
        % Delete points inside the polygons
        inObsIndex = inpolygon(Cnode(:,1),Cnode(:,2),...
                               poly_obs{i}.Vertices(:,1),...
                               poly_obs{i}.Vertices(:,2));
        Cnode(inObsIndex,:) = [];
    end
    RNodes = Cnode;
end

%% Calculate the Euclidean costs between two nodes
function euclid_cost = EuclideanCost(node1,node2)
    euclid_cost = sqrt((node1(1)-node2(1))^2 + (node1(2)-node2(2))^2);
end

%% Get the nearest node in 'nodes' from current x_samp
function near_ID = getNearNode(current_node,nodes,k,obstacles)
    N = size(nodes,1);
    costs = [];
    for i = 1:N
        x_near = [nodes(i,2) nodes(i,3)];
        IsFree = isCollisionFree(current_node,x_near,obstacles);
        if IsFree && EuclideanCost(current_node, [nodes(i,2) nodes(i,3)]) ~= 0
            costs = [costs; i EuclideanCost(current_node, [nodes(i,2) nodes(i,3)])];
        end
    end
    costs = sortrows(costs,2);
    near_ID = costs(1:k,1);
end

%% Check if the x_samp is 
%  1. inside the obstacle, or
%  2. has its edge with x_samp crossing the boundaries of the obstacle

%  Because we assume the robot to be a particle(i.e., its radius r = 0),
%  the radius of the obstacle R will grow to R+r to balance this assumption
%  However, the actual radius of the robot is not known. Therefore, we
%  define a factor of safety (FS) to enlarge the distance between the robot
%  and the boundary of the obstacle. FS in [0, 1], the larger the FS, the
%  more distant the robot is from the obstacle. 
%  FS = 0 => contact with obsticle's boundary
%  FS = 1 => 1 diameter (of the obsticle) away from the center of the 
%  obsticle

function IsFree = isCollisionFree(x_samp,x_near,obstacles)
    N = size(obstacles,1);
    FS = 0.10;
    IsFree = true;
    for i = 1:N
        center = [obstacles(i,1) obstacles(i,2)];
        diameter = obstacles(i,3);
        %****************check if coincides with the center****************
        if isequal(x_samp,center)
           IsFree = false; 
        end
        %******************check if inside the obstacle********************
        distance = EuclideanCost(x_samp, center);
        if distance <= (diameter / (2-FS) )
            IsFree = false;
        end
        %******************************************************************
        
        %*********check if edge crosses the obstacle's boundary************
        % Checking if the edge is blocked by the obstacle's boundary is the
        % same as checking if a line defined by two points intersects with
        % a circle defined by (center, radius). See this math stackexchange
        % discussion for detail:
        % https://math.stackexchange.com/questions/2837/how-to-tell-if-a-li
        % ne-segment-intersects-with-a-circle/2844#2844
        line = x_samp - x_near;
        normLine = line / norm(line);
        node2center = center - x_near;
        normal_distance = abs(node2center(1)*normLine(2) - ...
                              node2center(2)*normLine(1));
        if normal_distance <= (diameter / 2)
            PQ = EuclideanCost(x_samp,x_near);
            QC = EuclideanCost(x_samp,center);
            PC = EuclideanCost(x_near,center);
            cosPQC = (PC^2 - PQ^2 - QC^2) / (-2 * PQ * QC);
            cosQPC = (QC^2 - PQ^2 - PC^2) / (-2 * PQ * PC);
            if (cosPQC >= 0) && (cosQPC >= 0)
                IsFree = false;
            end
        end
        %******************************************************************        
    end
end

%% Gets the optimal path using A * Search, takes nodes and edges in form of
%  nodes
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  |       Column 4       |  
%  |  node ID   |     x      |     y      | heuristic-cost-to-go |
%  |    ....    |    ...     |    ...     |         ...          |
%  edges
%  # Comments
%  |  Column 1  |  Column 2  |  Column 3  |  
%  |  node ID1  |  node ID2  |  edge_cost |
%  |    ....    |    ...     |    ...     | 

function optPath = A_Star_Search(nodes,edges)
    N = size(nodes,1);      % get total No. of nodes
    goalSet = N;            % Assuming the last node is the goal node

    % Initialize Data Structures
    OPEN = [1];
    CLOSED = [];

    % Create cost matrix
    cost = createCostMatrix(edges,N); % cost(i,j) will return the cost of
                                      % going from node i to node j

    % Creaet minimal cost array
    myInf = max(edges(:,3)) * 100000;  % A really big number
    past_cost = [0 myInf.*ones(1,N-1)];% 0 cost to node 1, unknown for rest

    parent = zeros(1,N);         % A list storing the parent of the current 
    %% A star Algorithm
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
            if ~max(CLOSED==nbr(i))                  % if nbr not in CLOSED
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
end

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
%--------------------------------------------------------------------------

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```

### Results

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1ogqkExlGLLJsyc6DhZCW7sCX2RBgNI01" alt="fig_2.png">
</p>

Note that _N_ is the number of random nodes generated inside the given C-space.

***

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1Ns_O1sro0m_NgVlTO32nN0M74p-5fHDR" alt="fig_3.png">
</p>
Quelle: https://www.willowgarage.com/blog/2009/09/04/robot-comics-path-planning?page=9

***

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
