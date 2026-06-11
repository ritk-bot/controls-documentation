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