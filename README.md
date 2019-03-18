
## RRT Motion planner for 7 DOF Manipulator

THis project is part of a [Robotics](https://www.edx.org/course/robotics-2) course by Columbia University
 
In order for the robot end-effector to reach a desired pose without collisions with any obstacles present in its environment we need to implement motion planning. In this project we will code up a Rapidly-exploring Random Tree (RRT) motion planner for the 7-jointed robot arm. This will enable us to interactively maneuver the end-effector to the desired pose collision-free.

The arguments to the *motion_plan* function are:

* q_start: list of joint values of the robot at the starting position. This is the position in configuration space from which to start
* q_goal: list of joint values of the robot at the goal position. This is the position in configuration space for which to plan
* q_min: list of lower joint limits
* q_max: list of upper joint limits
*is_state_valid()* method is to check if a given point in configuration space is valid or causes a collision. The motion_plan function will return a path for the robot to follow. A path consists of a list of points in C-space that the robot must go through, where each point in C-space is in turn a list specifying the values for all the robot joints.

### Algorithm Overview:

The problem we are facing is to find a path that takes the robot from a give start to another end position without colliding with any objects on the way. This is the problem of motion planning. The RRT algorithm tackles this problem by placing nodes in configuration space at random and connecting them in a tree. Before a new node is added to the tree, the algorithm also checks if the path between them is collision free. *is_state_valid* function checks if a set of joint angles *q* creates a valid state, or one that is free of collisions. 
Once the tree reaches the goal position, we can find a path between start and goal by following the tree back to its root.
Once we find the path, we can move the end robot to the desired position using cartisian controller. The IK function will perform the inverse kinematics for a given transform T of the end-effector. It returns a list *q* of 7 values, which are the result positions for the 7 joints of the left arm, ordered from proximal to distal. If no IK solution is found, it returns an empty list.

The arguments to the *cartesian_control* function are:

* joint_transforms: a list containing the transforms of all the joints with respect to the base frame. In other words, joint_transforms[i] will contain the transform from the base coordinate frame to the coordinate frame of joint i.
* b_T_ee_current: current transform from the base frame to the end-effector
* b_T_ee_desired: desired transform from the base frame to the end-effector
The function must return a set of joint velocities such that the end-effector moves towards the desired pose.


![Output](https://github.com/sgottim2/csmm.103x_final/blob/master/output/output.gif)
