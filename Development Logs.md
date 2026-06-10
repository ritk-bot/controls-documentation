
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

The arm's shoulder is now operating through effort interfaces rather than direct position commands.

This establishes the foundation required for custom PID control development.
