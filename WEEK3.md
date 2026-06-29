# Week 3
## Instructions
### ROS2 Basics 
For Q1, Q2 and bonus questions your src shoulf follow the following structure.
#### Expected Package Structure
Question 1

Your workspace should contain the following package:

ros2_ws/
└── src/
    └── kratos_<your_name>/
        ├── CMakeLists.txt
        ├── package.xml
        ├── src/
        │   ├── rover_status_publisher.cpp or .py
        │   └── rover_status_subscriber.cpp or .py
        ├── launch/                 (Bonus)
        │   └── bringup.launch.py
        └── ...

Question 2

Create a second package for your custom message in bonus question.

ros2_ws/
└── src/
    ├── kratos_<your_name>/
    │   ├── CMakeLists.txt
    │   ├── package.xml
    │   ├── src/
    │   │   ├── rover_status_msg_publisher.cpp or .py
    │   │   └── rover_status_msg_subscriber.cpp or .py
    │   ├── launch/                 (Bonus)
    │   │   └── bringup.launch.py
    │   └── ...
    │
    └── kratos_<your_name>_msgs/
        ├── CMakeLists.txt
        ├── package.xml
        ├── msg/
        │   └── RoverStatus.msg
        └── ...

Replace <your_name> with your own name.

### IKFK and Transforms 

Q1 is a pen and paper question the answer to which is to be uploaded on Google Classroom.

For Q2, use the provided arm_humble package without changing its overall directory structure.

Your final package should resemble the following:

arm_humble/
├── launch/
├── rviz/
├── urdf/
├── scripts/
│   └── <script_name>.py
├── CMakeLists.txt
├── package.xml
└── ...

Requirements
Create a new scripts/ directory.
Place your ROS2 node inside the scripts/ directory.
Modify CMakeLists.txt so that your Python node is installed correctly. I have commented the required code block which you can comment out and edit.
If your implementation introduces additional dependencies, update package.xml accordingly.
Do not rename the package or modify the provided directory structure unless absolutely necessary. If necessary document such a change in your own repository's README.md.

## Documentation Requirements

Robotics projects are collaborative by nature. Your code should be understandable to another developer without requiring verbal explanations.
### Code Documentation
- Every custom function must include an appropriate docstring.
- Variable and function names should be descriptive.
