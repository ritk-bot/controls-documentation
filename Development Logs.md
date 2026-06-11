
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

[Screencast from 2026-06-11 15-33-12.webm](https://github.com/user-attachments/assets/2cbc2480-d6bb-4c2d-823d-2ba24f1edd96)

So we change our gains
```
self.kp = 5.0

self.ki = 0.0

self.kd = 2.0
```

The guide to tuning is in [PID Controller](PID%20Controller.md). Today i just tuned the shoulder joint and the params on my dummy arm were:
```
koro@victus:~/ros2_ws/src$ ros2 param dump /pid_controller
/pid_controller:
  ros__parameters:
    kd: 1.1
    ki: 0.3
    kp: 9.0
    start_type_description_service: true
    target: 0.3
    use_sim_time: false

```


I just got done learning PID now I'll be using the ros2_control method of implementing a PID controller where.
Instead of my YAML:

```
effort_controller:  type: forward_command_controller/ForwardCommandController
```

I will use:

```
shoulder_controller:  type: pid_controller/PidController
```

or a trajectory controller.

Then configure gains:

```
gains:  shoulder_joint:    p: 35.0    i: 0.1    d: 1.1
```

No custom Python node.

The controller i have made will help me while guiding inductees to make the hardware controller. 

Now we will be making a position controlled arm with ros2_control and JointTrajectoryController as the controller in my YAML
Steps:
- Change the URDF/Xacro command interfaces.
- Create a `JointTrajectoryController` YAML.
- Spawn the controller.
- Send a trajectory to the shoulder and elbow.
- Watch it move without my PID node.

	Teleop / Script/Moveit
        ↓
JointTrajectoryController
        ↓
	ros2_control
        ↓
	Gazebo 

is what i want.

We now have desired implementation and can use both teleop and moveit to control the arm urdf on gazebo.

