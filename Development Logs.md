
# 10th June 2026 Ritwik

- Started designing a PID controller for Milestone 4.
- modifying xacro to decide whether we wanna make a position controlled controller or an effort controlled one
- decided to go for effort controlled arm since that would allow for the smoothest move to hardware 
- Replaced command interface tags to effort in the xacro from position

![](Screenshot%20from%202026-06-11%2001-18-58.png) 

- Decision:

A separate PID development controller configuration will be created to allow direct effort command testing before integrating higher-level planning frameworks such as MoveIt.

Reason:

This approach isolates controller development and simplifies debugging of low-level control behavior.

- Forgot to generate arm.urdf from the xacro using 
  xacro urdf/my_custom_arm.urdf.xacro > urdf/arm.urdf
  Dont make this mistake its pretty stupid.
- Put this in your path if Gazebo throws an error related to plugins
	export GZ_SIM_SYSTEM_PLUGIN_PATH=/opt/ros/jazzy/lib:$GZ_SIM_SYSTEM_PLUGIN_PATH
