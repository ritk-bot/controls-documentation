
## Robotic Arm

## Documentation

- [Architecture](Architecture/Architecture.md)
- [PID Controller](Controls/PID%20Controller.md)
- [Development Logs](Development%20Logs/Development%20Logs.md)
- [Gazebo](Gazebo/Gazebo.md)
- [Milestones](Milestones/Milestones.md)
- [MoveIt](MoveIt/MoveIt.md)
- [Troubleshooting](Troubleshooting.md)


## Current Status

Currently on milestone 5. 6 and 7 to be done in college.

- Milestone 01 - Custom Arm URDF Development
- Milestone 02 - Gazebo and ros2_control Integration
- Milestone 03 - MoveIt Integration 
- Milestone 04 - PID Controller Development
- Milestone 05 - Genesis Integration
- Milestone 06 - Hardware Abstraction Layer
- Milestone 07 - STM32 / micro-ROS Backend

# How to Use This Documentation

This vault documents the development of the robotic arm controls subsystem.

The documentation is organized into several categories based on purpose.

---

# Documentation Structure

## Architecture

Contains the current design of the controls subsystem.

Use this section to answer:

- How does the system work today?
    
- What is the software architecture?
    
- What is the planned hardware architecture?
    
- How do MoveIt, ros2_control and the STM32 interact?
    

Read this section first if you are trying to understand the project.

---

## Milestones

Contains the development history of the project.

Use this section to answer:

- What was accomplished during each milestone?
    
- What problems were encountered?
    
- How were they solved?
    
- Why were certain design decisions made?
    

Each milestone acts as a project report documenting progress over time.

---

## Controls

Contains notes related to control systems.

Topics include:

- PID control
    
- PID tuning
    
- JointTrajectoryController
    
- ros2_control
    
- Control architecture experiments
    

Use this section when learning or implementing control algorithms.

---

## MoveIt

Contains documentation related to:

- MoveIt configuration
    
- Motion planning
    
- Controller integration
    
- Collision avoidance
    
- Troubleshooting
    

Use this section when working with robotic motion planning.

---

## Gazebo

Contains documentation related to:

- Simulation setup
    
- ros2_control integration
    
- Gazebo plugins
    
- Troubleshooting
    

Use this section when working with simulation.

---

## Development Logs

Contains daily engineering notes.

Examples:

- Debugging sessions
    
- Experiments
    
- Observations
    
- Temporary ideas
    
- Unresolved issues
    

Development logs are informal and may contain incomplete information.

Important findings should eventually be moved into the appropriate Architecture, Controls, MoveIt, Gazebo or Milestone document.

---

# Documentation Workflow

New information should flow through the documentation system as follows:

```text
Development Logs
        ↓
Investigation
        ↓
Milestone Report
        ↓
Architecture / Topic Documentation
```

Example:

```text
MoveIt execution bug
        ↓
Debugged in Development Logs
        ↓
Documented in Milestone 03
        ↓
Final solution added to MoveIt documentation
```

---

# Recommended Reading Order

For a new contributor:

1. Architecture
    
2. Latest Milestone
    
3. Controls
    
4. MoveIt
    
5. Gazebo
    

For troubleshooting:

1. Relevant subsystem documentation
    
2. Milestone reports
    
3. Development logs
    

For project history:

1. Milestone 01
    
2. Milestone 02
    
3. Milestone 03
    
4. Milestone 04
    
5. Future milestones
    

---

# Project Goal

The long-term objective is to develop a complete robotic arm software stack capable of operating in simulation and on physical hardware.

Planned roadmap:

Milestone 01 - Custom Arm Modeling ✓

Milestone 02 - Gazebo + ros2_control ✓

Milestone 03 - MoveIt Integration ✓

Milestone 04 - Control Architecture Exploration ✓

Milestone 05 - Genesis Integration

Milestone 06 - Hardware Abstraction Layer

Milestone 07 - STM32 + micro-ROS Backend

The final architecture is intended to support both simulation and real hardware with minimal software changes.