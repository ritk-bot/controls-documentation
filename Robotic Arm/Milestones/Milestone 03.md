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