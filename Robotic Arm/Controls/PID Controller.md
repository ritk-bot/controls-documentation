# Robotic Arm Control Architecture

## Purpose

This document summarizes the control architectures explored during development of the robotic arm and records the lessons learned from implementing both a custom PID controller and a ros2_control based solution.

---

# 1. Control Architecture Evolution

During development, two control approaches were explored:

## Approach 1: Custom PID Controller

```text
Target Position
       ↓
PID Controller Node
       ↓
Effort Commands
       ↓
Gazebo Joint
       ↓
Joint Feedback
       ↓
PID Controller
```

This architecture was implemented primarily for learning feedback control concepts such as:

- Proportional control
    
- Integral action
    
- Derivative damping
    
- Overshoot
    
- Steady-state error
    
- Controller tuning
    

Although educational, this architecture places all low-level control responsibilities inside a ROS node and does not scale well to larger systems.

---

## Approach 2: ros2_control + JointTrajectoryController

```text
Teleop / MoveIt
        ↓
JointTrajectoryController
        ↓
ros2_control
        ↓
Robot Hardware
```

In this architecture:

- High-level software specifies desired positions.
    
- JointTrajectoryController generates smooth trajectories.
    
- Low-level controllers are delegated to the hardware layer.
    

This is the architecture adopted for future development.

---

# 2. Final Planned Architecture

The intended hardware architecture is:

```text
Joystick / MoveIt
        ↓
Trajectory Generation
        ↓
JointTrajectoryController
        ↓
micro-ROS Transport
        ↓
STM32
        ↓
Joint PID Loops
        ↓
Motor Drivers
        ↓
Motors + Gearboxes
        ↓
Encoders
```

Responsibilities are separated across layers.

---

## High-Level Layer

Sources of commands:

- Keyboard teleoperation
    
- Joystick teleoperation
    
- MoveIt
    
- Autonomous software
    

These systems generate desired joint positions.

Example:

```text
Shoulder = 0.5 rad
Elbow    = -0.3 rad
```

---

## Trajectory Layer

The JointTrajectoryController:

- Interpolates between positions.
    
- Synchronizes multiple joints.
    
- Handles timing.
    
- Executes trajectories.
    

Example:

```text
Move shoulder from 0.3 rad
to 0.5 rad in 1 second
```

Internally:

```text
0.30
0.32
0.34
...
0.50
```

The controller generates intermediate setpoints automatically.

---

## Embedded Control Layer

Each STM32 joint controller executes:

```text
Desired Position
       ↓
Position Error
       ↓
PID Controller
       ↓
Motor Command
       ↓
Motor + Gearbox
       ↓
Encoder Feedback
```

The STM32 is responsible for real-time control and feedback processing.

---

# 3. Custom PID Controller Implementation

## Objective

Implement a reusable PID controller for understanding feedback control fundamentals.

The controller subscribes to:

```text
/joint_states
```

and publishes:

```text
/effort_controller/commands
```

The first implementation was performed on the shoulder joint only.

---

## PID Equation

```text
u = Kp·e + Ki∫e·dt + Kd·de/dt
```

where:

- e = position error
    
- Kp = proportional gain
    
- Ki = integral gain
    
- Kd = derivative gain
    

---

## Integral Windup Protection

The integral term accumulates error:

```python
integral += error * dt
```

To prevent excessive accumulation:

```python
integral = max(
    min(integral, 10.0),
    -10.0
)
```

Benefits:

- Reduces overshoot
    
- Prevents instability
    
- Prevents excessive control effort
    

---

## Effort Saturation

Controller output is limited:

```python
effort = max(
    min(effort, 50.0),
    -50.0
)
```

Benefits:

- Prevents unrealistic torques
    
- Improves simulation stability
    
- Protects future hardware
    

---

# 4. PID Tuning Procedure

## Step 1: Tune Kp

```text
Ki = 0
Kd = 0
```

Increase Kp until acceptable response speed is achieved.

---

## Step 2: Tune Kd

```text
Ki = 0
Kd > 0
```

Add damping to reduce overshoot.

---

## Step 3: Tune Ki

```text
Ki > 0
```

Reduce steady-state error.

---

## Runtime Tuning

```bash
ros2 param set /pid_controller kp 5.0
ros2 param set /pid_controller kd 2.0
ros2 param set /pid_controller ki 0.1
```

Target position:

```bash
ros2 param set /pid_controller target -1.0
```

Current parameters:

```bash
ros2 param dump /pid_controller
```

---

# 5. Teleoperation Strategy

## Current Method

Keyboard teleoperation generates position commands.

```text
Key Press
     ↓
Desired Joint Position
     ↓
JointTrajectoryController
```

---

## Future Joystick Method

Joystick axes naturally represent velocity commands.

Example:

```text
Axis = +0.5
```

The teleop node converts velocity into position:

```text
target_position += velocity × dt
```

This produces smooth continuous motion while remaining compatible with:

- JointTrajectoryController
    
- MoveIt
    
- STM32 position control
    

---

# 6. Key Lessons Learned

- PID control is useful for understanding feedback systems.
    
- JointTrajectoryController handles trajectory generation better than custom ROS-side effort controllers.
    
- MoveIt and teleoperation can share the same trajectory execution pipeline.
    
- Low-level PID control belongs on the embedded controller.
    
- High-level software should command positions rather than motor effort.
    
- The final architecture remains compatible with Gazebo, MoveIt and future STM32 hardware.