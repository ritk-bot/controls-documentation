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