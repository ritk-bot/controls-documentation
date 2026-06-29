# Week 3 - IK/FK and Transforms

## Question 1

Question 1 is a pen-and-paper question. Upload your scanned solution to **Google Classroom**.

---

## Question 2

Use the provided **arm_humble** package.

Do **not** rename the package or modify its overall directory structure unless absolutely necessary.

### Expected Package Structure

```text
arm_humble/
├── launch/
├── rviz/
├── urdf/
├── scripts/
│   └── ik_controller.py
├── CMakeLists.txt
├── package.xml
└── ...
```

### Requirements

- Create a new `scripts/` directory.
- Place your ROS2 node inside the `scripts/` directory.
- Modify `CMakeLists.txt` so your Python node is installed correctly.
- If additional ROS dependencies are required, update `package.xml`.
- Do not rename the package.

---

# Documentation Requirements

Your repository should demonstrate **both** a working solution and a clear engineering process.

## Code Documentation

Every custom function **must** contain an appropriate docstring.

Example:

```python
def inverse_kinematics(x, y):
    """
    Computes the shoulder and elbow joint angles required
    to reach a target end-effector position.

    Args:
        x (float): Target x coordinate.
        y (float): Target y coordinate.

    Returns:
        tuple: (shoulder_angle, elbow_angle)
    """
```

## Repository Documentation

Your repository must contain a `README.md` describing:

- Your approach to solving the problem.
- Any assumptions you made.
- Challenges encountered.
- How you tested your implementation.
- Any known limitations.

## Commit History

Commit your work regularly.

Avoid submitting your entire solution in a single commit.