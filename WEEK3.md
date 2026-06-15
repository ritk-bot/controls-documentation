# Kratos Mars Rover Club

## Controls Subsystem Induction Assignment 2026

---

# 1. Introduction to ROS 2

## What is ROS 2?

ROS 2 (Robot Operating System 2) is an open-source robotics middleware used to develop robotic systems. Despite its name, ROS 2 is not an operating system. Instead, it provides a collection of tools, libraries, and communication frameworks that allow different parts of a robot to communicate and work together efficiently. ROS 2 provides a standardized way for the components of the rover to exchange information and coordinate their actions.

## Core Concepts

### Nodes

A node is a single executable process that performs a specific task within a robotic system. Instead of creating one large program that controls the entire robot, ROS 2 encourages developers to divide functionality into multiple smaller nodes.

Examples of nodes include:

- Camera Node
    
- Navigation Node
    
- Arm Controller Node
    
- Battery Monitoring Node
    

### Publishers and Subscribers

Nodes rarely operate in isolation. In most robotic systems, nodes must exchange information with one another in order to perform meaningful tasks.

ROS 2 uses a **publish-subscribe communication model** to facilitate this exchange of information.

A node can **publish** data to a topic, while one or more nodes can **subscribe** to that topic and receive the published data.

For example:

- A camera node may publish images.
- A vision node may subscribe to those images and detect obstacles.
- A navigation node may subscribe to the obstacle information and plan a path.

```
Camera Node ---> /camera/image ---> Vision Node
```

This communication model allows robotic systems to remain modular, scalable, and easy to maintain.
### Messages and Message Types

Information exchanged between nodes is transmitted in the form of **messages**.

A message is a structured data object that contains information being sent from one node to another.

ROS 2 provides many predefined message types that are commonly used in robotics applications.

Some examples include:

|Message Type|Purpose|
|---|---|
|`std_msgs/String`|Text data|
|`std_msgs/Bool`|Boolean values|
|`std_msgs/Int32`|Integer values|
|`std_msgs/Float32`|Floating-point values|
|`geometry_msgs/Twist`|Velocity commands|
|`geometry_msgs/Pose`|Position and orientation data|

Selecting an appropriate message type is an important aspect of designing ROS systems.
### Topics

Topics allow nodes to exchange data using a publish-subscribe communication model.

A node can publish information on a topic, while one or more nodes can subscribe to that topic and receive the published data.

For example:

- A camera node publishes images on `/camera/image`
    
- A vision node subscribes to `/camera/image` and processes the images
    

### ROS Graphs

As robotic systems grow in complexity, understanding how nodes communicate becomes increasingly important.

ROS 2 provides a visualization tool called **rqt_graph** that displays the relationships between nodes and topics.

A simple ROS graph may look like:

```
Publisher Node ---> /chatter ---> Subscriber Node
```

### Services

Services provide request-response communication between nodes.

Unlike topics, which continuously stream data, services are used when one node needs to request a specific operation from another node and wait for a response.

For example:

- A node requests the current battery status
    
- The battery monitoring node responds with the requested information
    

### Actions

Actions are used for long-running tasks that require periodic feedback during execution.

Examples include:

- Moving a robotic arm to a target position
    
- Navigating to a specific location
    

Actions allow a client to monitor progress while the task is being executed and receive a result once it is complete.

It is recommended that you go through the above topics from this playlist to better understand how these components work:

https://youtube.com/playlist?list=PLLSegLrePWgJudpPUof4-nVFHGkB62Izy&si=hSVMhuG15rFiPk7j


---

# 2. Setting Up Your Workspace

Before beginning the assignment, ensure that ROS 2 Humble has been successfully installed on your system. 

# A Useful Tool: Terminator

Before making your workspace and packages, we strongly recommend installing **Terminator**, a terminal emulator that allows multiple terminal sessions to be viewed and managed within a single window.

ROS development often requires several terminals to be open simultaneously. For example, you may need one terminal for running a node, another for monitoring topics, and a third for building or debugging your workspace. Terminator makes this significantly more convenient by allowing terminals to be split both vertically and horizontally.

## Installation

Install Terminator using:

```
sudo apt update
sudo apt install terminator
```
### Useful Keyboard Shortcuts

| Shortcut         | Action                 |
| ---------------- | ---------------------- |
| Ctrl + Shift + O | Split Horizontally     |
| Ctrl + Shift + E | Split Vertically       |
| Ctrl + Shift + W | Close Current Terminal |
| Ctrl + Shift + T | Open New Tab           |
During development stuff often looks like this at 3AM. Hence having terminator helps a lot.

![](Assets/Pasted%20image%2020260615162242.png)


## Creating a ROS 2 Workspace
The official documentation: https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html

Create a workspace named `ros2_ws`:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```

Your workspace should now have the following structure:

```text
ros2_ws/
└── src/
```

## Creating Your Package
The official documentation : https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html

Navigate to the `src` directory and create a package using your name.

```bash
cd ~/ros2_ws/src

ros2 pkg create kratos_<your_name> --build-type ament_cmake 
```

Examples:

```bash
ros2 pkg create kratos_ritwik --build-type ament_cmake 
```

Your package name should follow the format:

```text
kratos_<your_name>
```

## Building the Workspace

Return to the workspace root:

```bash
cd ~/ros2_ws
colcon build
```

After building:

```bash
source install/setup.bash
```

To avoid sourcing the workspace every time a new terminal is opened, you may add the command above to your `.bashrc` using 
```
nano ~/.bashrc
```

## Recommended Package Structure

All assignment code should be placed inside your package.

The expected package structure is:

```text
ros2_ws/
└── src/
    └── kratos_<your_name>/
        ├── CMakeLists.txt
        ├── package.xml
        ├── include/
        │   └── kratos_<your_name>/
        ├── src/
        │   ├── q1_publisher.cpp
        │   ├── q1_subscriber.cpp
        │   ├── q2_telemetry.cpp
        │   └── q3_service.cpp
        └── README.md
```

### Folder Descriptions

|Folder/File|Purpose|
|---|---|
|`src/`|Source code for ROS nodes|
|`include/`|Header files|
|`launch/`|Launch files for running multiple nodes|
|`config/`|Parameter and configuration files|
|`docs/`|Additional documentation|
|`README.md`|Project overview and development logs|

## File Naming Convention

Use the following naming convention:

```text
q1_publisher.cpp
q1_subscriber.cpp
q2_telemetry.cpp
q3_service.cpp
```

The corresponding question number must be included in the filename.

## Running Your Nodes

After building and sourcing your workspace:

```bash
ros2 run <package_name> <executable_name>
```

Example:

```bash
ros2 run kratos_ritwik q1_publisher
```

## Verification Checklist

Before proceeding, ensure that:

-  ROS 2 Humble is installed successfully.
    
-  A workspace named `ros2_ws` has been created.
    
-  A package named `kratos_<your_name>` has been created.
    
-  The workspace builds successfully using `colcon build`.
    
-  The workspace has been sourced successfully.
    
-  You can execute a node using `ros2 run`.
    

---

# 3. Learning Resources

The following resources should be completed before attempting the assignment questions.

## ROS 2 Installation

Install ROS 2 Humble on Ubuntu 22.04:

[https://docs.ros.org/en/humble/Installation.html](https://docs.ros.org/en/humble/Installation.html)

## ROS 2 Tutorials

Beginner ROS 2 tutorials:

[https://docs.ros.org/en/humble/Tutorials.html](https://docs.ros.org/en/humble/Tutorials.html)

## Colcon Build System

Learn how ROS 2 packages are built:

[https://colcon.readthedocs.io](https://colcon.readthedocs.io)

## rqt_graph

Visualize communication between ROS nodes:

https://wiki.ros.org/rqt_graph

## Git and GitHub

If you are unfamiliar with Git and GitHub refer to submission guidelines

---

The following sections contain the assignment tasks. Ensure that you have completed the setup steps and reviewed the learning resources before proceeding.