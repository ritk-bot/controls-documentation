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