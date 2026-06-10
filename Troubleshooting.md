
- Use when gazebo sim throws an error related to plugins

		export GZ_SIM_SYSTEM_PLUGIN_PATH=/opt/ros/jazzy/lib:$GZ_SIM_SYSTEM_PLUGIN_PATH

  I recommend putting it in bashrc using
   
		nano ~/ .bashrc
	
- Sometimes one forgets to generate the urdf after modifying the xacro. One can check what ros2 and gazebo actually see using 

		grep command_interface install/arm/share/arm/urdf/arm.urdf

  Also the command to generate urdf is
  
		xacro urdf/my_custom_arm.urdf.xacro > urdf/arm.urdf
  
  for the given package.

- When debugging effort controllers, always verify:

	1. Controller activation.
	2. Interface claiming.
	3. Command topic publication.
	4. Robot pose and collision state.
	
	- Verify controller activation using:
	
	```
	ros2 control list_controllers
	```
	
	- Verify effort interfaces:
	
	```
	ros2 control list_hardware_interfaces
	```
	
	- Verify commands were reaching the controller:
	
	```
	ros2 topic echo /effort_controller/commands
	```
	
	- Check inertia values and joint limits.
	
	```
	ros2 topic echo /joint_states
	``` 
