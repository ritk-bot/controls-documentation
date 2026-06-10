
- Use when gazebo sim throws an error related to plugins

		export GZ_SIM_SYSTEM_PLUGIN_PATH=/opt/ros/jazzy/lib:$GZ_SIM_SYSTEM_PLUGIN_PATH

  I recommend putting it in bashrc using
   
		nano ~/ .bashrc
	
- Sometimes one forgets to generate the urdf after modifying the xacro. One can check what ros2 and gazebo actually see using 

		grep command_interface install/arm/share/arm/urdf/arm.urdf

  Also the command to generate urdf is
  
		xacro urdf/my_custom_arm.urdf.xacro > urdf/arm.urdf
  
  for the given package.