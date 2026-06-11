
# 10th June 2026 Ritwik

Started designing a PID controller for Milestone 4.


Modifying xacro to decide whether we wanna make a position controlled controller or an effort controlled one


Decided to go for effort controlled arm since that would allow for the smoothest move to hardware


Replaced command interface tags to effort in the xacro from position to effort

![](Assets/Screenshot%20from%202026-06-11%2001-18-58.png) 

Decision:

A separate PID development controller configuration will be created to allow direct effort command testing before integrating higher-level planning frameworks such as MoveIt.

Reason:

This approach isolates controller development and simplifies debugging of low-level control behavior.

## 11th June 2026 Ritwik

### Objective
Transition the arm from position-controlled joints to effort-controlled joints using ros2_control.

### Changes
- Modified ros2_control interfaces in the URDF/Xacro.
- Replaced position command interfaces with effort command interfaces.
- Created controllers_pid.yaml.
- Added ForwardCommandController configuration.
- Updated Gazebo ros2_control plugin configuration.
- Re-generated URDF from Xacro.
- Rebuilt workspace and relaunched simulation.

and then

- Updated `controllers_pid.yaml`.
- Added all arm joints to the `ForwardCommandController`.
- Controller now accepts a 6-element effort vector.
- Verified ros2_control effort interfaces are available and claimable.

### Verification

Controller activation:

effort_controller: activate successful

Hardware interfaces:

![](Assets/Screenshot%20from%202026-06-11%2002-17-42.png)

Other joints:

base_yaw_joint/effort
elbow_joint/effort
wrist_pitch_joint/effort
wrist_roll_joint/effort
gripper_servo_joint/effort

### Outcome

- Arm control architecture now supports whole-arm torque commands.
- Ready for development of the custom ROS 2 PID controller node

## 11th June 2026 Ritwik

Started work on PID controller. Made a new package arm_pid_controller. I will first be targetting only one joint (shoulder joint) and then extend to others.

Made the PID node executable and then built the package.

So the PID eqn is:

![](Assets/Pasted%20image%2020260611150903.png)

Problems with the first implementation: 

The shoulder oscillates a lot.

![](Assets/Screencast%20from%202026-06-11%2015-33-12.webm)