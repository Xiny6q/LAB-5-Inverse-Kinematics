Download link :https://programming.engineering/product/lab-5-inverse-kinematics/


# LAB-5-Inverse-Kinematics
LAB 5 Inverse Kinematics
5.1 Important

Read the entire lab before starting and especially the \Grading” section so you are aware of all due dates and requirements associated with the lab. Hope-fully you are reading this well before your lab section meets as given the com-pressed schedule, it is very important that you arrive at lab well prepared. This semester, the more you do prior to your lab session, the more you will get out of the short time you have with the TA.

5.2 Objectives

The purpose of this lab is to derive and implement a solution to the inverse kinematics problem for the UR3e robot. In this lab we will:

Derive elbow-up inverse kinematic equations for the UR3e

Write a Python function that moves the UR3e to a point in space speci ed by the user.

5.3 Reference

Chapter 6 of Modern Robotics provides multiple examples of inverse kinematics solutions.

5.4 Tasks

5.4.1 Solution Derivation

Make sure to read through this entire lab before you start deriving your solution. There are some needed details not covered in this


5.4. TASKS

section.

Given a desired end-e ector position in space (xgrip; ygrip; zgrip) and orientation f yaw; pitch(f ixed); roll(f ixed)g, write six mathematical expressions that yield values for each of the joint angles. For the UR3e robot, there are many solutions to the inverse kinematics problem. We will implement only one of the elbow-up solutions.

In the inverse kinematics problems you have examined in class (for 6 DOF arms with spherical wrists), usually the rst step is to solve for the coordi-nates of the wrist center. The UR3e does not technically have a spherical

wrist center but we will de ne the wrist center as zcen which equals the same desired z value of the suction cup and xcen, ycen are the coordinates of 6’s z axis. In addition, to make the derivation manageable, add that 5 will always be 90 and 4 is set such that link 7 and link 9 are always parallel to the world x,y plane.

Solve the inverse kinematics problem in the following order:

xcen, ycen, zcen, given yaw desired in the world frame and the desired x,y,z of the suction cup. The suction cup aluminum plate (link 9) has a length of 0.0535 meters from the center line of the suction cup to the center line of joint 6. Remember that this aluminum plate should always be parallel to the world’s x,y plane. See Figure 5.2.

1, by drawing a top down picture of the UR3e, Figure 5.1, and using xcen, ycen, zcen that you just calculated.

6, which is a function of 1 and yaw desired. Remember that when 6 is equal to zero the suction cup aluminum plate is parallel to link 4 and link 6.

x3end, y3end, z3end is a point o of the UR3e but lies along the link 6 axis, Figure 5.1. For example if 1 = 0 then y3end = 0. If 1 = 90 then x3end = 0. First use the top down view of the UR3e to nd x3end, y3end. One way is to choose an appropriate coordinate frame at xcen, ycen and nd the translation matrix that rotates and translates that coordinate frame to the base frame. Then nd the vector in the coordinate frame you chose at xcen, ycen that points from xcen, ycen to x3end, y3end. Simply multiply this vector by your translation matrix to nd the world coordinates at x3end, y3end. For z3end create a view of the UR3e, Figure 5.2, that is a projection of the robot onto a plane perpendicular to the x,y world frame and rotated by 1 about the base frame. Call this the side view. Looking at this side view you will see that z3end is zcen o set by a constant.

2, 3 and 4, by using the same side view drawing just drawn above to nd z3end, Figure 5.2. Now that x3end, y3end, z3end have been found use sine, cosine and the cosine rule to solve for partial angles that make up 2, 3 and 4. Hint: In this side view, a parallel to


5.4. TASKS


Figure 5.1: Top View Stick Pictorial of UR3e. Note that the coordinate frames are in the same direction as the World Frame but not at the World frame’s origin. One origin is along the center of joint 1 and the second is along the center of joint 6.


5.4. TASKS



5.5. PROCEDURE

the base construction line through joint 2 and a parallel to the base construction line through joint 4 are helpful in nding the needed partial angles.

5.4.2 Implementation

Implement the inverse kinematics solution by writing a Python function to re-ceive world frame coordinates (xW grip; yW grip; zW grip; yawW grip), compute the desired joint variables f 1; 2; 3; 4; 5; 6g, and command the UR3e to move to that pose using functions written in Lab4.

5.5 Procedure

Copy lab5pkg ik to your workspace. You will notice that there are three .py les. lab5 exec.py, lab5 func.py and lab5 header.py. The lab5 func.py le again will be compiled into a library so that future labs can easily call the inverse kinematic function. Like Lab 4, most of the needed code is given to you in lab5 exec.py. Your main job will be to add all the inverse kinematic equations to lab5 func.py. Please refer to the intermediate steps below to perform the inverse kinematic calcula-tions. If you look at lab5 header.py it includes lab5 header.py. This allows you to call the functions you created in lab5 func.py.

Once your code is nished, run it using \rosrun lab5pkg ik lab5 exec.py

[y] [z] [yaw(degrees)]” – e.g. \rosrun lab5pkg ik lab5 exec.py 0.1 0.1 0.15 90″. Remember to run drivers that in another command prompts at rst.

For in-person students, you should measure the x,y,z position of the end-e ector using the provided ruler and square. For students using the sim-ulation, it is not possible to measure this way, so we must use another method. A simple way is to use some of the ROS commands we learned before: \rostopic echo /gripper/position -n 1″. These values are be-ing calculated di erently and so there will be small di erences between this value and your calculations.

You should verify that your code works by selecting a variety of poses that will test the full range of motion. Your TA will not be providing you test points.

In your code (This is repeating the derivation steps above):

Establish the world coordinate frame (frame w) centered at the cor-ner of the UR3e’s base shown in Figure 5.3. The xw and yw plane should correspond to the surface of the table, with the xw axis par-allel to the sides of the table and the yw axis parallel to the front and back edges of the table. Axis zw should be normal to the table


5.5. PROCEDURE

surface, with up being the positive zw direction and the surface of the table corresponding to zw = 0.

We will solve the inverse kinematics problem in the base frame (frame 0), so we will immediately convert the coordinates entered by the user to base frame coordinates. Write three equations relating co-ordinates (xW grip; yW grip; zW grip) in the world frame to coordinates (xgrip; ygrip; zgrip) in the base frame of the UR3e.

xgrip(xW grip; yW grip; zW grip) =

ygrip(xW grip; yW grip; zW grip) =

zgrip(xW grip; yW grip; zW grip) =


Figure 5.3: Correct location and orientation of the world frame.

Given the desired position of the gripper (xgrip; ygrip; zgrip) (in the base frame) and the yaw angle, nd wrist’s center point (xcen; ycen; zcen).

xcen(xgrip; ygrip; zgrip; yaw) =

ycen(xgrip; ygrip; zgrip; yaw) =

zcen(xgrip; ygrip; zgrip; yaw) =


5.5. PROCEDURE

Given the wrist’s center point (xcen; ycen; zcen), write an expression for the waist angle 1. Make sure to use the atan2() function instead of atan() because atan2() takes care of the four quadrants the x,y coordinates could be in.

1(xcen; ycen; zcen) = (5.1)

4. Solve for the value of 6, given yaw and 1.

6( 1; yaw) = (5.2)

Find the projected end point (x3end; y3end; z3end) using (xcen; ycen; zcen) and 1.

x3end(xcen; ycen; zcen; 1) =

y3end(xcen; ycen; zcen; 1) =

z3end(xcen; ycen; zcen; 1) =

Write expressions for 2, 3 and 4 in terms of the end point. You probably will want to de ne some intermediate variables to help you with these calculations.

2(x3end; y3end; z3end) =

3(x3end; y3end; z3end) =

4(x3end; y3end; z3end) =

7. Now that your code solves for all the joint variables (remember that 5 is always 90 ) send these six values to the Lab 4 func-tion lab fk(). You will need to copy you Lab 4 solution into these functions. Do this simply to check that your inverse kinematic cal-culations are correct. Double check that the x,y,z point that you asked the robot to go to is the same value displayed by the forward kinematic equations.


5.6. REPORT

5.6 Report

You should submit a lab report using the guidelines given in the ECE 470: How to Write a Lab Report document. Please be aware of the following:

Lab reports are due with your demo for lab 5!

Lab reports will be submitted online at Blackboard. Your lab report should include the following:

A clearly written derivation of the inverse kinematics solution for each joint variable ( 1; 2; 3; 4; 5; 6). You must include gures in your derivation. Diagrams should be your own creation and clear and easily read. Do not use hand drawn gures or annotations.

For each test point include:

{ The given f xwgrip ; ywgrip ; zwgrip ; yawg

{ The measured position { The scalar error

Include a brief discussion of sources of error. As appendices to your report, include the following:

Your lab5 func.py code and lab5 exec.py if it was edited.

5.7 Demo

Your TA will require you to run your program twice, each time with a di erent set of desired position and orientation. Your program should reach the desired position and orientation with almost no error. You will be required to be able to demo on the simulator, even if you choose to demo on the real robot.

5.8 Grading

80 points, successful demonstration. 20 points, individual report.

