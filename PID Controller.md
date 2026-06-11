## Objective

Develop a reusable PID controller architecture for robotic arm joints.

## Design Requirements

- Independent of CAD geometry.
- Independent of simulator backend.
- Compatible with future STM32 integration.
- Compatible with MoveIt trajectories.

## Planned Architecture 

The custom package is of the architecture:

arm_pid_controller/
└── src/
    └── arm_pid_controller.py

Node subscribes to:

```
/joint_states
```

and publishes to:

```
/effort_controller/commands
```

The PID eqn is:

![](Assets/Pasted%20image%2020260611150942.png)

### My first implementation of the PID node (For learning and only on shoulder joint):

```
#!/usr/bin/env python3

  

import rclpy

  

from rclpy.node import Node

  

from sensor_msgs.msg import JointState

from std_msgs.msg import Float64MultiArray

  
  

class PIDController(Node):

  

def __init__(self):

  

super().__init__('pid_controller')

  

# Desired shoulder angle (radians)

self.target = 0.5

  

# Controller gains

self.kp = 10.0

self.ki = 0.5

self.kd = 0.1

  

self.integral = 0.0

  
  

self.prev_error = 0.0

self.prev_time = self.get_clock().now()

  

self.publisher = self.create_publisher(

Float64MultiArray,

'/effort_controller/commands',

10

)

  

self.subscription = self.create_subscription(

JointState,

'/joint_states',

self.joint_callback,

10

)

  

self.get_logger().info("PID Controller Started")

  

def joint_callback(self, msg):

  

if 'shoulder_joint' not in msg.name:

return

  

idx = msg.name.index('shoulder_joint')

  

position = msg.position[idx]

  

current_time = self.get_clock().now()

  

dt = (

current_time - self.prev_time

).nanoseconds / 1e9

  

if dt <= 0.0:

return

  

error = self.target - position

  

derivative = (error - self.prev_error) / dt

  

self.integral += error * dt

  

#Integral windup guard

self.integral = max(min(self.integral, 10.0), -10.0)

  

effort = (

self.kp * error +

self.ki * self.integral +

self.kd * derivative

)

  

# effort clamp

effort = max(min(effort, 50.0), -50.0)

  

cmd = Float64MultiArray()

  

cmd.data = [

0.0, # base_yaw

effort, # shoulder

0.0, # elbow

0.0, # wrist_pitch

0.0, # wrist_roll

0.0 # gripper

]

  

self.publisher.publish(cmd)

  

self.get_logger().info(

f"pos={position:.3f} "

f"err={error:.3f} "

f"effort={effort:.3f}"

)

  

self.prev_error = error

self.prev_time = current_time

  
  

def main():

  

rclpy.init()

  

node = PIDController()

  

rclpy.spin(node)

  

node.destroy_node()

  

rclpy.shutdown()

  
  

if __name__ == '__main__':

main()

```

### Integral Windup Guard

The PID controller maintains an integral term that accumulates the control error over time.

The integral term is updated using:

```
self.integral += error * dt
```

Without any limits, the integral value can grow indefinitely if the joint is unable to reach the target position. This phenomenon is known as **integral windup**.

When windup occurs:

- The accumulated integral becomes very large.
- The controller continues to command large efforts even after the error begins decreasing.
- The system may overshoot significantly or become unstable.

To prevent this, the integral term is clamped to a finite range:

```
self.integral = max(
    min(self.integral, 10.0),
    -10.0
)
```

This ensures that the accumulated error remains bounded and cannot dominate the controller output.

### Effort Clamp

The PID controller computes a desired effort using:

u = Kp·e + Ki·∫e·dt + Kd·de/dt

In some situations, particularly during startup or when large errors exist, the controller may request extremely large effort values.

Large effort commands can:

- Cause unrealistic behaviour in simulation.
- Produce violent oscillations.
- Destabilize the physics engine.
- Damage real hardware in future implementations.

To prevent this, the controller output is limited:

```
effort = max(
    min(effort, 50.0),
    -50.0
)
```

This bounds the maximum torque that can be commanded by the controller.

Note: 

	This code is only for the shoulder joint for now.

### Code with changeable params:

```
#!/usr/bin/env python3

  

import rclpy

  

from rclpy.node import Node

  

from sensor_msgs.msg import JointState

from std_msgs.msg import Float64MultiArray

  
  

class PIDController(Node):

  

def __init__(self):

  

super().__init__('pid_controller')

self.declare_parameter("kp", 10.0)

self.declare_parameter("ki", 0.5)

self.declare_parameter("kd", 0.1)

self.declare_parameter("target", 0.5)

  

# Desired shoulder angle (radians)

self.target = self.get_parameter("target").value

  

# Controller gains

self.kp = self.get_parameter("kp").value

self.ki = self.get_parameter("ki").value

self.kd = self.get_parameter("kd").value

  

self.integral = 0.0

  
  

self.prev_error = 0.0

self.prev_time = self.get_clock().now()

  

self.publisher = self.create_publisher(

Float64MultiArray,

'/effort_controller/commands',

10

)

  

self.subscription = self.create_subscription(

JointState,

'/joint_states',

self.joint_callback,

10

)

  

self.get_logger().info("PID Controller Started")

  

def joint_callback(self, msg):

  

self.kp = self.get_parameter("kp").value

self.ki = self.get_parameter("ki").value

self.kd = self.get_parameter("kd").value

  

self.target = self.get_parameter("target").value

if 'shoulder_joint' not in msg.name:

return

  

idx = msg.name.index('shoulder_joint')

  

position = msg.position[idx]

  

current_time = self.get_clock().now()

  

dt = (

current_time - self.prev_time

).nanoseconds / 1e9

  

if dt <= 0.0:

return

  

error = self.target - position

  

derivative = (error - self.prev_error) / dt

  

self.integral += error * dt

  

#Integral windup guard

self.integral = max(min(self.integral, 10.0), -10.0)

  

effort = (

self.kp * error +

self.ki * self.integral +

self.kd * derivative

)

  

# effort clamp

effort = max(min(effort, 50.0), -50.0)

  

cmd = Float64MultiArray()

  

cmd.data = [

0.0, # base_yaw

effort, # shoulder

0.0, # elbow

0.0, # wrist_pitch

0.0, # wrist_roll

0.0 # gripper

]

  

self.publisher.publish(cmd)

  

self.get_logger().info(

f"pos={position:.3f} "

f"err={error:.3f} "

f"effort={effort:.3f}"

)

  

self.prev_error = error

self.prev_time = current_time

  
  
  

def main():

  

rclpy.init()

  

node = PIDController()

  

rclpy.spin(node)

  

node.destroy_node()

  

rclpy.shutdown()

  
  

if __name__ == '__main__':

main()
```


## PID Tuning Procedure

The PID controller was tuned using a step-by-step approach rather than modifying all gains simultaneously.

### Step 1: Tune the Proportional Gain (P)

Initially, only the proportional gain was enabled.

```text
Ki = 0
Kd = 0
```

The proportional gain was gradually increased until the joint responded quickly to target changes.

Observations:

- Low Kp resulted in slow movement.
    
- High Kp caused oscillations and overshoot.
    
- The objective was to find the largest Kp that still produced stable behaviour.
    

---

### Step 2: Add Derivative Gain (D)

Once a reasonable proportional gain was found, derivative control was introduced.

```text
Ki = 0
Kd > 0
```

The derivative term acts as damping by opposing rapid changes in error.

Observations:

- Increasing Kd reduced oscillations.
    
- Excessive Kd slowed the response.
    
- The objective was to achieve fast motion with minimal overshoot.
    

---

### Step 3: Add Integral Gain (I)

After obtaining stable PD behaviour, a small integral gain was introduced.

```text
Ki > 0
```

The integral term removes steady-state error by accumulating error over time.

Observations:

- Increasing Ki reduced residual position error.
    
- Excessive Ki caused overshoot and oscillation.
    
- Integral windup became a concern for large errors.
    

To prevent windup, the accumulated integral value was clamped:

```python
self.integral = max(min(self.integral, 10.0), -10.0)
```

---

### Recommended Tuning Order

```text
1. Tune Kp
2. Tune Kd
3. Tune Ki
```

Avoid changing multiple gains simultaneously during tuning.

---

### Runtime Tuning

Controller gains can be modified while the PID node is running.

Examples:

```bash
ros2 param set /pid_controller kp 5.0
ros2 param set /pid_controller kd 2.0
ros2 param set /pid_controller ki 0.1
```

Target positions can also be changed during runtime:

```bash
ros2 param set /pid_controller target -1.0
```

This allows rapid experimentation without rebuilding or restarting the node.

To know what the params are set to currently,
```
ros2 param dump /pid_controller
```

---

### Metrics Observed During Tuning

The following characteristics were monitored:

- Rise Time: Time taken to approach the target position.
    
- Overshoot: Maximum amount by which the joint exceeded the target.
    
- Settling Time: Time required for oscillations to diminish.
    
- Steady-State Error: Final difference between target and actual position.
    

The goal was to minimize overshoot and settling time while maintaining acceptable response speed.


# Future Control Architecture

## Motivation

The custom Python PID controller was implemented to understand the fundamentals of feedback control, including proportional, integral and derivative action, overshoot, damping and steady-state error.

While this approach is valuable for learning and prototyping, a larger robotic system benefits from separating high-level motion generation from low-level motor control.

---

## Layered Control Architecture

The final robotic arm is expected to use a layered control structure.

```text
Joystick / MoveIt
        ↓
Trajectory Generation
        ↓
JointTrajectoryController
        ↓
STM32 Joint Controllers
        ↓
Motor Drivers
        ↓
Motors + Gearboxes
        ↓
Joint Encoders
```

Each layer has a specific responsibility.

---

## High-Level Control

High-level control determines where the robot should move.

Possible sources include:

- Joystick teleoperation
    
- Keyboard teleoperation
    
- MoveIt motion planning
    
- Autonomous software
    
- Vision-based control
    

These systems do not command motor torque directly.

Instead, they generate desired joint positions or trajectories.

Example:

```text
Shoulder Joint = 0.5 rad
Elbow Joint = -0.3 rad
```

---

## JointTrajectoryController

The JointTrajectoryController is responsible for trajectory execution.

Its responsibilities include:

- Interpolating between target positions
    
- Generating smooth motion
    
- Synchronizing multiple joints
    
- Enforcing timing constraints
    

For example, a command such as:

```text
Move shoulder from 0.3 rad to 0.5 rad in 1 second
```

may internally become:

```text
0.30
0.32
0.34
0.36
0.38
0.40
...
0.50
```

The controller continuously produces intermediate setpoints that define the desired trajectory.

Importantly, the JointTrajectoryController does not directly control motor torque.

---

## Embedded Joint Controllers

The STM32 is responsible for low-level control.

For each joint:

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

The encoder provides the current joint position.

The PID controller calculates the error between the desired position and measured position and generates the required motor command.

This process executes continuously at a high frequency.

---

## Example: Shoulder Joint Motion

Assume the shoulder is currently at:

```text
0.30 rad
```

The operator pushes the joystick forward.

The teleoperation node requests:

```text
0.50 rad
```

The JointTrajectoryController generates intermediate setpoints:

```text
0.32
0.34
0.36
...
0.50
```

The STM32 receives the current desired position and computes:

```text
error = desired_position - measured_position
```

The PID controller then drives the motor until the measured position matches the desired position.

---

## Velocity-Based Teleoperation

For teleoperation, joystick axes naturally correspond to velocity commands rather than absolute positions.

Example:

```text
Joystick Forward
→ Shoulder Velocity = +0.3 rad/s
```

The teleoperation node integrates velocity into position targets:

```text
target_position += velocity × dt
```

This produces smooth continuous motion while still using the same trajectory execution and low-level control infrastructure.

---

## Relationship Between PID and JointTrajectoryController

The JointTrajectoryController and PID controller solve different problems.

JointTrajectoryController:

- Determines where the joint should be at a given time.
    
- Handles trajectory timing and synchronization.
    

PID Controller:

- Ensures the physical joint reaches the desired position.
    
- Compensates for disturbances, gravity and model inaccuracies.
    

Therefore:

```text
JointTrajectoryController
        =
Trajectory Execution

PID Controller
        =
Motor Control
```

Both layers are required for a complete robotic manipulation system.

