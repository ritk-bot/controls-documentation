
# Milestone 01 - Custom Robotic Arm Modeling

## Objective

Create a robotic arm model from scratch for learning ROS2, MoveIt, and robotic manipulation concepts.

## Motivation

The mechanical subsystem had not yet completed the CAD design for the rover arm.

A temporary arm model was created to develop and validate the software stack independently of mechanical progress.

## Work Completed

- Designed a multi-joint robotic arm using URDF/Xacro.
    
- Defined links and joints manually.
    
- Created kinematic chain structure.
    
- Configured joint limits.
    
- Configured collision geometry.
    
- Configured visual geometry.
    

Joint Structure:

- base_yaw_joint
    
- shoulder_joint
    
- elbow_joint
    
- wrist_pitch_joint
    
- wrist_roll_joint
    
- gripper_servo_joint
    

## Outcome

Successfully created a fully functional robotic arm description package without requiring CAD models.

## Lessons

Software development for robotics can begin before mechanical designs are finalized.

A simplified arm model can be used to validate:

- Motion planning
    
- Controller architecture
    
- Simulation workflows
    
- ROS2 integration


# Milestone 02 - Gazebo and ros2_control Integration

## Objective

Create a simulated robotic arm capable of receiving commands and moving inside Gazebo through ros2_control.

## Initial State

At the beginning of this milestone:

- URDF was functional.
    
- Arm could be visualized in RViz.
    
- No controller infrastructure existed.
    
- No simulation backend existed.
    

## Tasks Completed

### Gazebo Integration

- Added Gazebo support to the arm package.
    
- Created simulation launch files.
    
- Verified successful spawning of the arm inside Gazebo.
    

### ros2_control Integration

Added ros2_control support to the robot description.

Configured:

- controller_manager
    
- joint_state_broadcaster
    
- arm_controller
    

Created:

- ros2_controllers.yaml
    
- ros2_control.xacro configuration
    

### Keyboard Teleoperation

Implemented keyboard teleoperation for testing controller functionality.

This provided a simple method to command joints before introducing MoveIt.

## Problems Encountered

### Arm Did Not Move

Symptoms:

- Controllers appeared to load.
    
- Gazebo launched correctly.
    
- Teleop commands produced no motion.
    

Investigation:

- Verified controller startup.
    
- Verified ros2_control configuration.
    
- Verified controller_manager status.
    

Root Cause:

Incorrect ros2_control configuration and controller definitions.

Resolution:

Updated controller configuration and reloaded controllers.

### Joint States Not Published

Symptoms:

- MoveIt and RViz could not determine current robot state.
    
- State updates were missing.
    

Investigation:

Checked:

- joint_state_broadcaster
    
- state interfaces
    
- controller loading sequence
    

Root Cause:

joint_state_broadcaster was not correctly configured and activated.

Resolution:

Corrected broadcaster configuration and verified /joint_states publication.

### Controller Interface Mismatch

Symptoms:

Controllers loaded but failed to command joints correctly.

Investigation:

Verified:

- command interfaces
    
- state interfaces
    
- controller configuration
    

Resolution:

Aligned controller definitions with ros2_control interfaces.

## Outcome

Successfully achieved:

Keyboard Teleop  
→ ros2_control  
→ Gazebo  
→ Robotic Arm Motion

This milestone established the complete simulation and control infrastructure used by all future milestones.

# Milestone 03 - MoveIt Integration

## Objective

Enable motion planning and trajectory execution for the robotic arm using MoveIt.

## Initial State

At the beginning of this milestone:

- URDF operational.
    
- Gazebo operational.
    
- ros2_control operational.
    
- Keyboard teleop functional.
    

The arm could be manually controlled but could not perform autonomous planning.

## Tasks Completed

### MoveIt Configuration

Generated:

- SRDF
    
- kinematics.yaml
    
- moveit_controllers.yaml
    
- planning configuration
    

Configured planning groups for the robotic arm.

### MoveIt Execution Pipeline

Connected:

MoveIt  
→ arm_controller  
→ ros2_control  
→ Gazebo

## Problems Encountered

### Planning Worked But Execution Failed

Symptoms:

- MoveIt generated valid trajectories.
    
- Robot visualization displayed planned path.
    
- Execute button failed.
    

Investigation:

Verified:

- controller status
    
- MoveIt controller configuration
    
- ros2_control configuration
    

Root Cause:

MoveIt could not properly communicate with trajectory controllers.

Resolution:

Updated:

- moveit_controllers.yaml
    
- ros2_controllers.yaml
    
- controller mappings
    

### Joint State Synchronization Issues

Symptoms:

- MoveIt reported state inconsistencies.
    
- Execution occasionally failed.
    

Investigation:

Verified:

- /joint_states publication
    
- robot_state_publisher
    
- joint_state_broadcaster
    

Root Cause:

State information was not consistently available to MoveIt.

Resolution:

Corrected broadcaster configuration and controller startup sequence.

### Trajectory Execution Reliability

Symptoms:

Planning occasionally failed on first attempt but succeeded on subsequent attempts.

Investigation:

Observed controller initialization and planning scene updates.

Resolution:

Controller and planning scene synchronization improved after startup stabilization.

## Additional Validation

### Collision Avoidance Testing

Added collision objects to the planning scene.

Verified:

- Obstacle insertion
    
- Collision checking
    
- Alternate path generation
    

Observed MoveIt successfully generating collision-free trajectories around obstacles.

## Outcome

Successfully achieved:

MoveIt  
→ Trajectory Planning  
→ Trajectory Execution  
→ ros2_control  
→ Gazebo

The arm transitioned from manual teleoperation to autonomous motion planning.

# Milestone 4

## Initial Architecture Review

Before implementing the PID controller, the existing robotic arm control architecture was reviewed.

Findings:

The current arm configuration utilized position-based control interfaces.

Each joint exposed:

- position command interface
    
- position state interface
    
- velocity state interface
    

This architecture allowed trajectory execution through MoveIt but bypassed low-level control behavior.

Decision:

The PID controller milestone will migrate the arm toward effort-based control.

Rationale:

Effort control more closely resembles the eventual hardware architecture:

MoveIt  
→ PID Controller  
→ STM32  
→ Motor Driver  
→ Encoder Feedback

This allows simulation behavior to more closely match future hardware behavior and reduces architectural changes required during hardware integration.

The existing robotic arm utilized a position-controlled architecture.

Controller Configuration:

- JointTrajectoryController
- Position command interfaces
- Position and velocity state interfaces

Observation:

The existing control stack bypassed low-level actuator behavior and directly commanded joint positions.

Implication:

A custom PID controller could not be inserted meaningfully into the control pipeline while using position command interfaces.

Decision:

Migrate the arm toward effort-based control interfaces before PID implementation.

Reason:

Effort control more closely resembles the eventual hardware architecture:

MoveIt  
→ PID Controller  
→ STM32  
→ Motor Driver  
→ Encoder Feedback

This minimizes future architectural changes when transitioning from simulation to hardware.