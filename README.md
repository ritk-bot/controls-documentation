# Controls Documentation

Documentation for the custom robotic arm project.

## Current Progress on the Arm

- [x] Milestone 01 - Arm Modeling
- [x] Milestone 02 - Gazebo + ros2_control
- [x] Milestone 03 - MoveIt Integration
- [x] Milestone 04 - Control Architecture Exploration
- [ ] Milestone 05 - Genesis
- [ ] Milestone 06 - Hardware Abstraction Layer
- [ ] Milestone 07 - STM32 + micro-ROS

## Documentation

- [Robotic Arm](Robotic%20Arm/Robotic%20Arm.md)
- [Life Detection Mechanism](Life%20Detection%20Mechanism/Life%20Detection%20Mechanism.md)


# Git Branch Workflow

## Check Current Branch

```bash
git branch --show-current
```

or

```bash
git status
```

---

## Create and Switch to a New Branch

```bash
git switch -c milestone4-pid
```

Equivalent older command:

```bash
git checkout -b milestone4-pid
```

---

## Verify Branch

```bash
git branch
```

Example:

```text
main
* milestone4-pid
```

The `*` indicates the currently active branch.

---

## Make Changes and Commit

Check the repository status:

```bash
git status
```

Stage all changes:

```bash
git add .
```

Commit the changes:

```bash
git commit -m "First changes for PID controller"
```

---

## Push Branch to GitHub (First Time)

```bash
git push -u origin milestone4-pid
```

The `-u` flag:

- Creates the remote branch on GitHub.
    
- Sets it as the upstream tracking branch.
    

---

## Future Pushes

After the first push:

```bash
git push
```

is sufficient.

---

# Useful Commands

## See All Local Branches

```bash
git branch
```

## See Local and Remote Branches

```bash
git branch -a
```

---

## Switch Branches

Switch to main:

```bash
git switch main
```

Switch to another branch:

```bash
git switch milestone4-pid
```

---

## Unstage All Added Files

If you accidentally ran:

```bash
git add .
```

Unstage everything:

```bash
git restore --staged .
```

Alternative:

```bash
git reset
```

---

## Check Branch Status

```bash
git status
```

Example:

```text
On branch milestone4-pid
Your branch is up to date with 'origin/milestone4-pid'.

nothing to commit, working tree clean
```

---

# Handling Push Rejected Errors

If:

```bash
git push
```

fails because the remote contains newer commits:

```bash
git pull --rebase
git push
```

To make rebase the default behavior:

```bash
git config --global pull.rebase true
```

---

# Typical Milestone Workflow

```bash
git switch main
git pull

git switch -c milestone4-pid

# Work on the project

git add .
git commit -m "Implement PID controller"

git push -u origin milestone4-pid
```

Continue development:

```bash
git add .
git commit -m "Tune PID gains"
git push
```

---

# Merging Into Main

Once the milestone is complete:

1. Push the latest changes.
    
2. Open GitHub.
    
3. Click **Compare & Pull Request**.
    
4. Create a Pull Request.
    

```text
milestone4-pid -> main
```

5. Review and merge into `main`.
    
