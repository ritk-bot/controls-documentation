## Objective

Develop a reusable PID controller architecture for robotic arm joints.

## Design Requirements

- Independent of CAD geometry.
- Independent of simulator backend.
- Compatible with future STM32 integration.
- Compatible with MoveIt trajectories.

## Planned Architecture

MoveIt/Joystick
↓
PID Controller
↓
ros2_control
↓
Simulator / Hardware

