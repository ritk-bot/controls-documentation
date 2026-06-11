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

My first implementation of the PID node:

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

