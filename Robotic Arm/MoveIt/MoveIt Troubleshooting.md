- If plan execution fails on gazebo then usually moveit clock and gazebo clocks are not synced.  So we use 
	```ros2 param set /move_group use_sim_time true```
	if 
  ```ros2 param get /move_group use_sim_time```
	gives false.
 
