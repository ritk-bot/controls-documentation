# Robotic Arm Milestones

This section tracks the development roadmap of the robotic arm subsystem.

Each milestone represents a major engineering objective and records the progress, implementation details, and lessons learned during development.

---

## Development Roadmap

### Milestone 1 — Custom Arm URDF Development

Objectives:

- Design robot structure
- Define links and joints
- Create URDF model
- Verify kinematic chain

Status: Completed

---

### Milestone 2 — Gazebo and ros2_control Integration

Objectives:

- Integrate robot into Gazebo
- Configure ros2_control
- Implement controller management
- Enable keyboard teleoperation

Status: Completed

---

### Milestone 3 — MoveIt Integration

Objectives:

- Generate MoveIt configuration
- Configure planning groups
- Integrate trajectory execution
- Execute trajectories in simulation

Status: Completed

---

### Milestone 4 — PID Controller Development

Objectives:

- Implement custom PID controller
- Compare against JointTrajectoryController
- Evaluate controller performance
- Understand feedback control principles

Status: In Progress

---

### Milestone 5 — Genesis Integration

Objectives:

- Integrate Genesis simulation framework
- Evaluate simulation capabilities
- Compare with Gazebo workflows

Status: Planned

---

### Milestone 6 — Hardware Abstraction Layer

Objectives:

- Design hardware-independent interfaces
- Separate software from hardware implementations
- Prepare deployment architecture

Status: Planned

---

### Milestone 7 — STM32 / micro-ROS Backend

Objectives:

- Integrate STM32 hardware
- Implement micro-ROS communication
- Validate end-to-end control pipeline

Status: Planned

---

## How to Use This Section

Each milestone should contain:

- Objectives
- Architecture
- Implementation Details
- Testing Results
- Lessons Learned
- Future Improvements

The milestone documents collectively describe the complete development journey of the robotic arm subsystem.