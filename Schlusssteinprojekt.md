---
layout: post
mathjax: true
title: 'Schlusssteinprojekt'
description: My attemp for the Schlusssteinprojekt
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='KapXIII.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="alert('This is the very last chapter!')">Next</button></span>
</p>

# Capstone Project

## Main script: capstone_main.m

```Matlab
%% This script produces csv files that will guide a youbot (4 mecanum whee
%  -ls chassis + UR5 arm) to perform pick-drop action. The script takes
%  four input, namely:
%  1. initial configuration of the end-effector in the form of
%     [chassis phi, x, y, J1, J2, J3, J4, J5, W1, W2, W3, W4]
%  2. location of cube-to-pick:
%     [x, y, theta]
%  3. goal location to drop the cube:
%     [x, y, theta]
%  4. Error gains:
%     [Kp, Ki]
%=========================================================================%
%  For more information about this assignment, please visit:
%  http://hades.mech.northwestern.edu/index.php/Mobile_Manipulation_Capstone
%=========================================================================%
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020/04/03
%=========================================================================%
%                     The Program Begins Here.
%                   　　　　　　_.，------..
%                   　　　　　／　　　　　`＂_
%                   　　　　ノ　　　　　　　　\
%                   　　　ノ　　　　　　　　　　\
%                   　　　|　　／\　　／\　　　　|
%                   　　　|　　　　　　　　　　｜
%                   　　　\　　（_人_）　　　　/
%                   　....—＼　　　　　　　＿／———.
%                   |"　　　　' "_　　　　..,-＇　'|
%                   ＇￣￣￣＼　　　　　　　／￣￣￣＇
%                   　　　　　|　　　　　　　|
%                   　　　　　\　　　|'￣＼｀　|
%                   　　　　　＼　 　\　　　／
%                   　　　　　　＼　　\_＿／
%                   　　　　　　　＼_＿、\
%=========================================================================%


%--------------------------------------------------------------------------
%                              Constants
%--------------------------------------------------------------------------
%% Kinematic Model of the youBot
%  For detail information, please consult #Kinematics of the youBot section
%  of the page: 
%  http://hades.mech.northwestern.edu/index.php/Mobile_Manipulation_Capstone
% Fixed offset from chassis frame {b} to base frame of arm {0}: Tb0
T_b0 = [1 0 0 0.1662;
        0 1 0    0  ;
        0 0 1 0.0026;
        0 0 0    1  ;];
    
% When arm at home config, end-effector frame {e} relative to arm base 
%  frame {0}, i.e., the home transformation matrix M0e is
M_0e = [1 0 0 0.033 ;
        0 1 0   0   ;
        0 0 1 0.6546;
        0 0 0   1   ;];
    
% Body screw axes list, Blist
B1 = [0 0 1 0 0.033 0]';
B2 = [0 -1 0 -0.5076 0 0]';
B3 = [0 -1 0 -0.3526 0 0]';
B4 = [0 -1 0 -0.2176 0 0]';
B5 = [0 0 1 0 0 0]';
BList = [B1 B2 B3 B4 B5];

% Chassis model
l = 0.47/2;
w = 0.3/2;
r = 0.0475;

% create moRobo Object
myRobo = youbotKUKA(l, w, r, 'KUKA youbot with UR5 end-effector', ...
                    T_b0, M_0e, BList);
fprintf('Successfully created the youbot kinematic model!\n')
disp('myRobo')

%--------------------------------------------------------------------------
%                               User Input
%--------------------------------------------------------------------------
%% Asking for input
fprintf('***=========================================================================***\n')
fprintf('Please fill in the following initial variables so as to perform the simulation.\n')

% initial configuration of end-effector initial_T_se
[initial_T_se, initial_Config] = getInitialConfiguration(M_0e,BList,T_b0);

% initial and final cube location, initial_T_sc & goal_T_sc
initial_T_sc = getInitialCubeLocation();

goal_T_sc = getGoalCubeLocation();

% compute standoff and gripping HTM
T_ce_standoff = getCEStandoffHTM();
T_ce_gripping = getCEGrippingHTM();

% reference initial configuration to test the feedback control
% because input a matrix in Command Window is troublesome, this value is
% hard-coded here
ref_initial_T_se = [ 0 0 1   0;
                     0 1 0   0;
                    -1 0 0 0.5;
                     0 0 0   1];
ini_Err = HTM2pose(ref_initial_T_se) - HTM2pose(initial_T_se);

% feedback gains
gains = getControlGains();

%% Reference end-effector configuration
initial_T_standoff = initial_T_sc * T_ce_standoff;
T_grasp = initial_T_sc * T_ce_gripping;
goal_T_standoff = goal_T_sc * T_ce_standoff;
T_release = goal_T_sc * T_ce_gripping;

% Arranged in a way so that it can be looped in the 'Steps' section
Configurations = {ref_initial_T_se, initial_T_standoff, T_grasp, T_grasp,...
                  initial_T_standoff, goal_T_standoff, T_release, T_release...
                  goal_T_standoff};
    
%--------------------------------------------------------------------------
%                      Generate reference trajectory
%--------------------------------------------------------------------------
%% Generate reference trajectory
delt_t = 0.01;      % 10ms interval
motionTime = 13;    % total motion time
Tf = computeTf(motionTime, Configurations);

% 8 Steps
% 1. A trajectory to move the gripper from its initial configuration to
%    a "standoff" configuration a few cm above the block.
% 2. A trajectory to move the gripper down to the grasp position.
% 3. Closing of the gripper.
% 4. A trajectory to move the gripper back up to the "standoff" 
%    configuration.
% 5. A trajectory to move the gripper to a "standoff" configuration 
%    above the final configuration.
% 6. A trajectory to move the gripper to the final configuration of the 
%    object.
% 7. Opening of the gripper.
% 8. A trajectory to move the gripper back to the "standoff" 
%    configuration.

grpOPEN = 1;
grpCLOSE = 0;
methodOrder = 5;    % quintic polynomials
gripperStatus = 0;

% trajectory in form of 
% [r11 r12 r13 r21 r22 r23 r31 r32 r33 Xpos Ypos Zpos gripperStatus]
traj = []; 

for ii = 1 : 8
    if ii == 3
        % gripperStatus is a BOOL-type variable, in which
        % 1 = gripper closed
        % 0 = gripper opened
        gripperStatus = grpOPEN;  
    elseif ii == 7
        gripperStatus = grpCLOSE;
    end
    temp_traj = myRobo.trajectoryGenerator(Configurations{ii}, ...
                                           Configurations{ii+1}, ...
                                           Tf(ii), ...
                                           delt_t, ...
                                           gripperStatus, ...
                                           methodOrder);
    traj = [traj; temp_traj];
    % the above trajectory of the end-effector is used later to guide the
    % motion of the KUKA youbot
end

%--------------------------------------------------------------------------
%                           Feedback PI controller
%--------------------------------------------------------------------------
%% PI Feedback control
myRobo.q = [initial_Config(1) initial_Config(2) initial_Config(3)];

kp = gains(1);
myRobo.Kp = kp * eye(6);
ki = gains(2);
myRobo.Ki = ki * eye(6);

myRobo.theta = [0 0 0.2 -1.67 0]';
myRobo.wheelAngle = [0 0 0 0];

speedLimit = 12.3 * ones(1,9);
jointLimit = [ones(1,5) * pi; ones(1,5) * (-pi)];

[NHTM, NgripperStatus] = NTraj2NHTM(traj);

[scene6simulation, Xerr] = myRobo.feedbackController(delt_t,NHTM,...
                                                     speedLimit,jointLimit,...
                                                     NgripperStatus);
writematrix(scene6simulation,'scene6simulation.csv','Delimiter','comma');

plot(Xerr','LineWidth',1.5);
title('$X_{err}$ against time','FontSize',12,'FontWeight','bold',...
      'interpreter', 'latex')
xlabel('Time (10ms)','FontSize',12,'FontWeight','bold');
ylabel('Error Twist','FontSize',12,'FontWeight','bold');
legend('$W_x$','$W_y$','$W_z$','$V_x$','$V_y$','$V_z$',...
       'interpreter','latex')
grid on;
writematrix(Xerr,'Xerr.csv','Delimiter','comma');
 
            
%--------------------------------------------------------------------------
%                       Functions are defined below
%--------------------------------------------------------------------------
%% getInitialConfiguration
%  ask the user to input the initial configuration of the robot in array
%  form and then compute based on the input the HTM of the initial 
%  configuration of end-effector: ini_T_se
%  For testing, type:
%  [0.6, 0.0, 0.0, 0.0, -0.35, -0.698, -0.505, 0.0, 0.0, 0.0, 0.0, 0.0]
function [ini_T_se, iniConfig] = getInitialConfiguration(M_0e,BList,T_b0)
    prompt1 = ['\n 1. Please enter the initial configuration, Tse, of the\n'...
              'end-effector in the form of\n'...
              '[phi, x, y, J1, J2, J3, J4, J5, W1, W2, W3, W4] :\n'];
    iniConfig = input(prompt1);

    % generate transformation matrix for the end-effector from input config
    R_vec = [0 0 iniConfig(1)];
    R_so3 = VecToso3(R_vec);
    R = MatrixExp3(R_so3);
    p = [iniConfig(2) iniConfig(3) 0.0963]';
    T_sb = RpToTrans(R,p);
    T_0e = FKinBody(M_0e, BList, iniConfig(4:8)');
    T_be = T_b0 * T_0e;
    ini_T_se = T_sb * T_be;
end

%% getInitialCubeLocation
%  ask the user to input the initial location of the cube-to-pick in array
%  form and then compute based on the input the HTM of the cube
%  configuration.
%  For testing, type [1, 0, 0]
function ini_T_sc = getInitialCubeLocation()
    prompt2 = ['\n 2. Please enter the initial location of the cube-to-pick in the\n'...
               'form of [x, y, theta] : \n'];
    temp = input(prompt2);
    x = temp(1);
    y = temp(2);
    theta = temp(3);
    rot = [0 0 theta]';
    pos = [x y 0.025]';
    R_so3 = VecToso3(rot);
    R = MatrixExp3(R_so3);
    ini_T_sc = RpToTrans(R,pos);
end

%% getGoalCubeLocation
%  ask the user to input the initial location of the cube-to-drop in array
%  form and then compute based on the input the HTM of the cube
%  configuration
%  For testing, type [0, -1, -pi/2]
function goal_T_sc = getGoalCubeLocation()
    prompt3 = ['\n 3. Please enter the goal location of the cube-to-drop in the\n'...
               'form of [x, y, theta] : \n'];
    temp = input(prompt3);
    x = temp(1);
    y = temp(2);
    theta = temp(3);
    rot = [0 0 theta]';
    pos = [x y 0.025]';
    R_so3 = VecToso3(rot);
    R = MatrixExp3(R_so3);
    goal_T_sc = RpToTrans(R,pos);
end

%% getControlGains
%  ask the user to input the feedback controller's gains, namely the ki and
%  kp
function gains = getControlGains()
    prompt4 = ['\n 4. Please enter the feedback controller gains in\n'...
               'the form of [kp, ki] : \n'];
    gains = input(prompt4);
end

%% getCEStandoffHTM
%  get standoff HTM
function T_ce_standoff = getCEStandoffHTM()
    theta = [0 pi/5+pi/2 0]';
    R_so3 = VecToso3(theta);
    R = MatrixExp3(R_so3);
    pos = [0 0 0.250]';
    T_ce_standoff = RpToTrans(R,pos);
end

%% getCEGrippingHTM
%  get gripping HTM
function T_ce_gripping = getCEGrippingHTM()
    theta = [0 pi/5+pi/2 0]';
    R_so3 = VecToso3(theta);
    R = MatrixExp3(R_so3);
    pos = [0 0 0]';
    T_ce_gripping = RpToTrans(R,pos);
end

%% Convert a HTM to pose ([Rx Ry Rz x y z]')
function pose = HTM2pose(HTM)
    [R, p] = TransToRp(HTM);
    R_so3 = MatrixLog3(R);
    rot = so3ToVec(R_so3);
    pose = [rot; p];
end

%% Convert a traj to HTM
%  convert a traj in form of
%  N * [r11 r12 r13 r21 r22 r23 r31 r32 r33 Xpos Ypos Zpos gripperStatus]
%  into a 3x3xN tensor containing the HTM and a 1xN array containing the
%  gripperStatus
function [NHTM, NgripperStatus] = NTraj2NHTM(Ntraj)
    N = size(Ntraj,1);
    NHTM = zeros(4,4,N);
    NgripperStatus = zeros(1,N);
    for ii = 1 : N
        r1j = Ntraj(ii,1:3);
        r2j = Ntraj(ii,4:6);
        r3j = Ntraj(ii,7:9);
        R = [r1j; r2j; r3j];
        p = Ntraj(ii,10:12)';
        temp_HTM = RpToTrans(R,p);
        NHTM(:,:,ii) = temp_HTM;
        NgripperStatus(ii) = Ntraj(ii,13);
    end
end

%% Compute weighted time Tf using Euclidean distance
function Tf = computeTf(motionTime, Configurations)
    % Initialize variables
    T_se_initial = Configurations{1};
    T_standoff_initial = Configurations{2};
    T_grasp = Configurations{3};
    T_standoff_final = Configurations{6};
    T_release = Configurations{7};

    % for debugging
    %fprintf('T_se_initial:\n')
    %disp(T_se_initial)
    %fprintf('T_standoff_initial:\n')
    %disp(T_standoff_initial)
    %fprintf('T_grasp:\n')
    %disp(T_grasp)
    %fprintf('T_standoff_final:\n')
    %disp(T_standoff_final)
    %fprintf('T_release:\n')
    %disp(T_release)

    normalized_D = [norm(T_se_initial(1:3,4)-T_standoff_initial(1:3,4)),...
                    norm(T_standoff_initial(1:3,4)-T_grasp(1:3,4)),...
                    0,...
                    norm(T_standoff_initial(1:3,4)-T_grasp(1:3,4)),...
                    norm(T_standoff_initial(1:3,4)-T_standoff_final(1:3,4)),...
                    norm(T_standoff_final(1:3,4)-T_release(1:3,4)),...
                    0,...
                    norm(T_standoff_final(1:3,4)-T_release(1:3,4))];
    D_sum = sum(normalized_D);

    % 8 Steps
    % 1. A trajectory to move the gripper from its initial configuration to
    %    a "standoff" configuration a few cm above the block.
    % 2. A trajectory to move the gripper down to the grasp position.
    % 3. Closing of the gripper.
    % 4. A trajectory to move the gripper back up to the "standoff" 
    %    configuration.
    % 5. A trajectory to move the gripper to a "standoff" configuration 
    %    above the final configuration.
    % 6. A trajectory to move the gripper to the final configuration of the 
    %    object.
    % 7. Opening of the gripper.
    % 8. A trajectory to move the gripper back to the "standoff" 
    %    configuration.
    
    % N.B. that each of step 3 & 7 should consist of at least 63 identical 
    % lines of the csv file in which the gripper state matches previous 
    % line in the csv file, so as to make sure that the gripper motion is 
    % completed.
    
    t = zeros(1,8);
    gripperRunningTime = 0.63;      % 630 ms, 63 of 10ms intervals
    for i = 1:8
        if (i == 3) || (i == 7)
            t(i) = gripperRunningTime;            
        else
            t(i) = normalized_D(i) * (motionTime - 2 * gripperRunningTime) / ...
                 D_sum;
        end
    end

    Tf = round(t * 10^2) / 10^2;    % round into multiples of 10ms 
end
```

## superclass: wheeledMobileRobot.m

```Matlab
%% This file defines a super class called 'wheeledMobileRobot'.
%  This file is part of the capstone project of the Coursera MOOC Modern
%  Robotics Course 6. 
%  Written by Donglin Sui (lordblackwoods@gmail.com) on 2020/04/05
%  In MATLAB, the relation between a subclass and a super class is like
%  the relation between 'Desk' and 'Furniture', in which the later contains
%  the generic information of certain object, and the former contains
%  (usually) more specific pieces of information. For readers who are not
%  familiar with OOP (Object-Oriented-Programming), check this Q&A :
%  https://www.mathworks.com/matlabcentral/answers/89957-why-subclass-superclass-in-oop
classdef wheeledMobileRobot
    properties
        % kinematic model
        l           % length from center of car to wheel axis in x_b 
                    % direction, the half_length
        w           % length from center of car to wheel in y_b direction, 
                    % the half_width
        r           % radius of wheel
        type        % robot type, in this case, the type is 'youBot'
        
        % for feedback PI control
        q           % in the form of [chassis_phi x y]
        q_dot
        Vb
        u
        
        % odometry
        wheelAngle
    end
    
    properties (Constant)
        height = 0.0963 
    end
    
    properties (Dependent)
        config
        H_0
    end
    
    methods
        function obj = set.u(obj,val)
             obj.u = val;
        end
        
        % The following three functions are used for the dependent
        % properties
        function temp = get.config(obj)
           theta = obj.q(1);
           R = [cos(theta) -sin(theta) 0;
                sin(theta)  cos(theta) 0;
                    0           0      1];
           x = obj.q(2);
           y = obj.q(3);
           z = obj.height;
           p = [x y z]';
           temp = RpToTrans(R,p);
        end
        
        function temp = get.H_0(obj)
            rr = obj.r;
            ll = obj.l;
            ww = obj.w;
            temp = [-(ll+ww)/rr 1/rr -1/rr;
                     (ll+ww)/rr 1/rr  1/rr;
                     (ll+ww)/rr 1/rr -1/rr;
                    -(ll+ww)/rr 1/rr  1/rr;];
        end
        
        % This function allows the creation of kinematic model of chassis
        function obj = wheeledMobileRobot(half_length,half_width,...
                                          radius,roboType)
            obj.l = half_length;
            obj.w = half_width;
            obj.r = radius;
            obj.type = roboType;
        end
        
        function obj = nextState(obj, u, delt_t, speedLimit)
            u(find((u>=speedLimit) == 1)) = 12.3;
            
            % Odometry
            % Vb = H(0)^{dagger} * delta_theta = F * delta_theta
            delta_theta = u';
            obj.Vb = pinv(obj.H_0) * delta_theta;
            
            % chassis twist
            omega_bz = obj.Vb(1);
            v_bx = obj.Vb(2);
            v_by = obj.Vb(3);
            % 6-dimensional version of the planar chassis twist 
            vb6 = [0 0 omega_bz v_bx v_by 0]';
            % integrate vb6 over delt_t so as to generate the displacement
            % created by the wheelAngle increment
            vb6_integral = vb6 * delt_t;
            vb6_se3mat = VecTose3(vb6_integral);
            T_bb_dash_SE3 = MatrixExp6(vb6_se3mat);
            T_k = obj.config;
            T_k_next = T_k * T_bb_dash_SE3;
            
            [R, p] = TransToRp(T_k_next);
            R_so3 = MatrixLog3(R);
            rot = so3ToVec(R_so3);
            chassis_phi_next = rot(3);
            x_next = p(1);
            y_next = p(2);
            
            obj.q = [chassis_phi_next x_next y_next];
            obj.wheelAngle = obj.wheelAngle + u*delt_t;
            
                
            %T_bk_bknext = MatrixExp6(VecTose3(vb6*delt_t));
            %T_s_bknext = obj.config*T_bk_bknext;

            %obj.q = [atan2(T_s_bknext(2,1), T_s_bknext(1,1)),T_s_bknext(1,4),T_s_bknext(2,4)];
            %obj.wheelAngle = obj.wheelAngle + u*delt_t;
            
       end
        
    end
    
    
end
```

## subclass: youbotKUKA.m

```Matlab
%% Subclass of 'wheeledMobileRobot'
classdef youbotKUKA < wheeledMobileRobot
    properties
        % for kinematic model
        M_be
        Blist
        
        % trajectory
        Trajectory
        
        % for feedback PI control
        theta
        theta_dot
        Kp
        Ki
        
        % for forward kinematic
        T_be
        T_se
    end
    
    properties (Dependent)
        Je
    end
    
    methods
        function Je = get.Je(obj)
            J_arm = JacobianBody(obj.Blist,obj.theta);
            F3 = pinv(obj.H_0, 0.0001);
            F6 = zeros(6,4);
            F6(3:5,:) = F3;
            T_eb = TransInv(obj.T_be);
            Ad_T_eb = Adjoint(T_eb);
            J_base = Ad_T_eb * F6;
            Je = [J_base J_arm];
        end
        
        function obj = youbotKUKA(half_length, half_width, radius,...
                                  roboType, T_b0, M_0e, BList)
            obj@wheeledMobileRobot(half_length, half_width, radius, ...
                                   roboType);
            obj.M_be = T_b0 * M_0e;
            obj.Blist = BList;
        end
        
        % This function shuffles a HTM into array form
        % [r11 r12 r13 r21 r22 r23 r31 r32 r33 Xpos Ypos Zpos gripperStatus]
        function formattedTraj = shuffleTrajectory(obj,gripperStatus)
            len = size(obj.Trajectory,2);
            formattedTraj = zeros(len,13);
            for ii = 1 : len
                temp_HTM = obj.Trajectory{ii};
                r1j = temp_HTM(1,1:3);
                r2j = temp_HTM(2,1:3);
                r3j = temp_HTM(3,1:3);
                p = temp_HTM(1:3,4)';
                formattedTraj(ii,:) = [r1j, r2j, r3j, p, gripperStatus];
            end
        end
        
        function traj = trajectoryGenerator(obj, Xstart, Xend, T, delt_t,...
                                            gripperStatus, methodOrder)
            N = round(T/delt_t + 1);
            obj.Trajectory = CartesianTrajectory(Xstart, Xend, T, N, ...
                                                 methodOrder);
            % shuffle the trajectory into the output format, which is 
            % [r11 r12 r13 r21 r22 r23 r31 r32 r33 Xpos Ypos Zpos gripperStatus]
            %  rij = terms in the rotation section of the HTM
            %  Xpos = x-coordinate in the p section of the Rp HTM
            %  gripperStatus is a boolean value defined as true = closed, false = open
            formattedTraj = shuffleTrajectory(obj,gripperStatus);
            traj = round(formattedTraj, 14);    % this rounding digit is 
                                                % chosen arbitrarily and 
                                                % can be reduced to 8
        end
        
        % forward kinematics
        function obj = FK(obj)
            temp = FKinBody(obj.M_be, obj.Blist, obj.theta);
            obj.T_be = temp;
            obj.T_se = obj.config * temp;
        end
        
        function obj = nextState(obj,config,speed,delt_t,speedLimit)
            obj = nextState@wheeledMobileRobot(obj,speed(1:4),delt_t,speedLimit(1:4));
            % using logical matrix to index which element the speed is
            % bigger than speedLimit
            speed = speed(5:9);
            speedLimit = speedLimit(5:9);
            speed(find((speed>=speedLimit)==1)) = 12.3;
            obj.theta = config(4:8) + speed * delt_t; 
            obj.theta = obj.theta';
       end
        
        function [scene6simulation, Xerr, obj] = feedbackController(obj,...
                                                 delt_t,NHTM,...
                                                 speedLimit,jointLimit,...
                                                 NgripperStatus)
            N = size(NHTM,3);
            scene6simulation = zeros(N,13);
            Xerr = zeros(6,N-1);
            V = zeros(6,N-1);
            for ii = 1 : (N-1)
                obj = obj.FK;
                T_se_temp = obj.T_se;
                obj.T_be = FKinBody(obj.M_be, obj.Blist, obj.theta);
                
                % initialize terms required by eqn. (13.37) of the textbook
                Xd = NHTM(:,:,ii);
                Xd_inv = TransInv(Xd);
                Xd_next = NHTM(:,:,ii+1);
                Td_next = Xd_inv * Xd_next;
                Vd_se3 = MatrixLog6(Td_next);
                Vd_se3 = Vd_se3 / delt_t;
                Vd = se3ToVec(Vd_se3);
                T_es_temp = TransInv(T_se_temp);
                T_err = T_es_temp * Xd;
                xerr_se3 = MatrixLog6(T_err);
                xerr = se3ToVec(xerr_se3);
                Xerr(:,ii) = xerr;
                XerrSum = sum(Xerr,2); 
                Ad_Terr = Adjoint(T_err);
                
                % now apply eqn. (13.37)
                V(:,ii) = Ad_Terr * Vd + obj.Kp * xerr + obj.Ki * XerrSum ...
                                                                  * delt_t;
                % the commanded end-effector-frame twist is thus
                % implemented as commanded_Twist = J^{dagger}_{e} * V
                commanded_Twist = pinv(obj.Je, 0.0090) * V(:,ii);
                obj.u = commanded_Twist(1:4);
                obj.theta_dot = commanded_Twist(5:9);
                commanded_Twist = [obj.u' obj.theta_dot'];
                config = [obj.q obj.theta' obj.wheelAngle];
                
                % Joint limit test
                Je_new = jointLimitCheck(obj, jointLimit, commanded_Twist, ...
                                         config, delt_t, speedLimit);
                commanded_Twist = pinv(Je_new,0.0090) * V(:,ii);
                obj.u = commanded_Twist(1:4);
                obj.theta_dot = commanded_Twist(5:9);
                commanded_Twist = [obj.u' obj.theta_dot'];
                
                obj = obj.nextState(config, commanded_Twist, delt_t, ...
                                    speedLimit); 
                scene6simulation(ii+1,:) = [obj.q ...
                                            obj.theta' ...
                                            obj.wheelAngle ...
                                            NgripperStatus(ii)];
            end
        end
        
        function Je = jointLimitCheck(obj, jointLimit, speed, config, ...
                                      delt_t, speedLimit)
           obj_test = obj.nextState(config,speed,delt_t,speedLimit);
           No_joint = size(jointLimit,2);
           Je = obj.Je;
           theta_test = obj_test.theta';
           for i = 1 : No_joint
               if theta_test(i) > jointLimit(1,i)||theta_test(i) < jointLimit(2,i)
                   Je(:,i+4) = zeros(6,1);
               end   
           end
        end
        
    end
    
end
```

***

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
