
# Autonomous Line Tracking Robot Using ROS and Computer Vision

## Project Overview

This project focuses on implementing **autonomous line tracking** for a mobile robot using ROS and computer vision. The main objective was to enable the robot to detect a colored line on the ground and follow it without manual control.

The robot used its onboard RGB camera to capture real-time images of the environment. The camera image was processed using HSV color segmentation to isolate the target line color from the surrounding floor. After detecting the line, the program calculated the horizontal offset between the center of the line and the center of the camera frame. This offset was then used as an error signal to adjust the robot’s steering while maintaining forward motion.

This project demonstrates how vision-based feedback control can be used to guide an autonomous mobile robot using visual cues from its environment.

---

## Objectives

- Implement autonomous line following using ROS.
- Use the robot’s RGB camera for real-time visual feedback.
- Convert camera images into HSV color space for line detection.
- Tune HSV parameters to isolate the target line color.
- Calculate line position relative to the camera frame center.
- Use steering correction to keep the robot aligned with the line.
- Tune robot response using `rqt_reconfigure`.
- Validate autonomous line tracking through live testing and video demonstration.

---

## Tools and Technologies Used

- ROS
- Python
- Yahboom Robot Platform
- RGB Camera
- Computer Vision
- HSV Color Segmentation
- `rqt_reconfigure`
- ROS Launch Files
- `cmd_vel` Motion Control
- PID-style Steering Parameters
- Linux Terminal

---

## Methodology and Project Execution

The project was completed through a structured ROS-based computer vision and motion control workflow. The overall process included launching the robot system, activating the camera interface, selecting the target line color, tuning HSV parameters, and testing autonomous line following.

### 1. Robot Bringup

The first step was to start the ROS master and launch the robot bringup system.

```bash
roscore
````

In a second terminal, the workspace was sourced and the robot bringup launch file was started:

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch yahboomcar_bringup bringup.launch
```

This initialized the robot hardware, communication nodes, and required ROS interfaces.

---

### 2. Located and Prepared the Line Following Script

The line-following script was located inside the ROS workspace.

```bash
cd ~/catkin_ws/src
find . -name follow_line.py
cd ~/catkin_ws/src/yahboomcar_linefollow/scripts
nano follow_line.py
chmod +x follow_line.py
```

The script `follow_line.py` was responsible for processing the camera image, detecting the line, calculating the error, and sending motion commands to the robot.

---

### 3. Launched the Line Following System

After preparing the script, the line-following launch file was executed.

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch yahboomcar_linefollow follow_line.launch VideoSwitch:=true img_flip:=false
```

The launch command activated the camera window and started the line detection system.

---

### 4. Camera-Based Color Selection

In the camera window, the following keyboard controls were used:

| Key     | Function                    |
| ------- | --------------------------- |
| `R`     | Enter color selection mode  |
| `I`     | Enter target detection mode |
| `Space` | Start line following        |
| `Q`     | Stop the robot              |

The target line color was selected by pressing `I` and clicking/holding on the colored line region in the camera window. This allowed the robot to identify the specific color range it needed to follow.

---

### 5. HSV Color Detection

The camera image was converted from RGB/BGR format into HSV color space. HSV color space was used because it separates color information from brightness, making it easier to detect the target line under different lighting conditions.

The main HSV parameters used for tuning were:

| Parameter | Meaning            | Purpose                           |
| --------- | ------------------ | --------------------------------- |
| `Hmin`    | Minimum Hue        | Lower bound of target color       |
| `Hmax`    | Maximum Hue        | Upper bound of target color       |
| `Smin`    | Minimum Saturation | Filters weak or gray colors       |
| `Smax`    | Maximum Saturation | Controls maximum color intensity  |
| `Vmin`    | Minimum Brightness | Removes dark pixels and shadows   |
| `Vmax`    | Maximum Brightness | Removes overly bright reflections |

By tuning these parameters, the robot was able to isolate the line from the floor and background.

---

### 6. Tuning Robot Response Using RQT Reconfigure

The robot’s tracking behavior was tuned using `rqt_reconfigure`.

```bash
rosrun rqt_reconfigure rqt_reconfigure
```

The tuning window allowed real-time adjustment of line detection and control parameters.

Important parameters included:

| Parameter      | Purpose                                                           |
| -------------- | ----------------------------------------------------------------- |
| `Kp`           | Controls how strongly the robot turns when the line is off-center |
| `Ki`           | Corrects long-term steering drift                                 |
| `Kd`           | Smooths steering by reacting to error changes                     |
| `linear`       | Sets forward speed                                                |
| `LaserAngle`   | Defines LiDAR detection angle                                     |
| `ResponseDist` | Sets obstacle response distance                                   |
| `scale`        | Adjusts processed image size                                      |

During testing, the proportional gain was adjusted:

```text
Kp changed from 60 to 90
```

This improved the robot’s steering response when the line moved away from the center of the camera frame.

---

## How the System Works

The autonomous line tracking system follows a feedback-control loop:

```text
Camera Captures Image
        ↓
Image Converted to HSV Color Space
        ↓
Target Line Color Is Segmented
        ↓
Line Center Is Detected
        ↓
Error = Line Center - Image Center
        ↓
Angular Velocity Is Adjusted
        ↓
Robot Moves Forward While Correcting Direction
```

If the line appears to the left of the camera center, the robot turns left. If the line appears to the right, the robot turns right. If the line is centered, the robot continues moving forward.

This loop repeats continuously, allowing the robot to follow the line autonomously.

---

## Results and Outcomes

The project successfully demonstrated autonomous line tracking using ROS and computer vision.

Major outcomes included:

* Activated the robot camera interface successfully.
* Detected a colored line using HSV color segmentation.
* Tuned HSV parameters to isolate the selected line color.
* Used the line offset from the camera center as a steering error.
* Integrated visual feedback with robot motion control.
* Tuned the proportional gain from **60 to 90** for better steering response.
* Enabled the robot to follow the line autonomously after pressing the space bar.
* Validated the result through a short video demonstration.

---

## Key Features

* Real-time camera-based line detection
* HSV color segmentation
* Interactive color selection
* Autonomous line following
* ROS-based motion control
* Dynamic parameter tuning using `rqt_reconfigure`
* PID-style steering correction
* Forward motion with angular correction
* Live camera visualization
* Simple keyboard control interface

---

## Project Workflow

```text
Start ROS Master
        ↓
Launch Robot Bringup
        ↓
Locate and Prepare follow_line.py
        ↓
Launch Line Following Node
        ↓
Open Camera Window
        ↓
Select Target Line Color
        ↓
Tune HSV Parameters
        ↓
Tune Kp, Ki, Kd, and Speed
        ↓
Press Space to Start Following
        ↓
Robot Tracks Line Autonomously
```

---

## Commands Used

### Terminal 1: Start ROS Master

```bash
roscore
```

### Terminal 2: Launch Robot Bringup

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch yahboomcar_bringup bringup.launch
```

### Terminal 3: Locate and Run Line Following Script

```bash
cd ~/catkin_ws/src
find . -name follow_line.py
cd ~/catkin_ws/src/yahboomcar_linefollow/scripts
nano follow_line.py
chmod +x follow_line.py
source ~/catkin_ws/devel/setup.bash
roslaunch yahboomcar_linefollow follow_line.launch VideoSwitch:=true img_flip:=false
```

### Terminal 4: Open Dynamic Reconfigure

```bash
rosrun rqt_reconfigure rqt_reconfigure
```

---

## Parameter Tuning Summary

| Parameter      | Description                | Effect on Robot                        |
| -------------- | -------------------------- | -------------------------------------- |
| `Hmin`         | Minimum hue                | Sets lower color threshold             |
| `Hmax`         | Maximum hue                | Sets upper color threshold             |
| `Smin`         | Minimum saturation         | Removes dull colors                    |
| `Smax`         | Maximum saturation         | Controls color intensity range         |
| `Vmin`         | Minimum brightness         | Removes shadows/dark regions           |
| `Vmax`         | Maximum brightness         | Removes glare/bright noise             |
| `Kp`           | Proportional gain          | Controls turning strength              |
| `Ki`           | Integral gain              | Reduces long-term drift                |
| `Kd`           | Derivative gain            | Smooths sudden steering changes        |
| `linear`       | Forward speed              | Controls robot speed                   |
| `LaserAngle`   | LiDAR angle range          | Defines obstacle detection range       |
| `ResponseDist` | Obstacle response distance | Defines when robot reacts to obstacles |

---

## Demo

A short video demonstration was recorded showing the robot following the selected colored line autonomously.

```text
visual.mp4
```

---

## Major Learnings

Through this project, I gained practical experience with:

* ROS robot bringup and launch files
* Camera-based robot perception
* HSV color segmentation
* Visual feedback control
* Line tracking logic
* Dynamic parameter tuning using `rqt_reconfigure`
* Robot steering control using image error
* Real-time testing and debugging of autonomous robot behavior

---

## Conclusion

This project successfully implemented autonomous line tracking using ROS and computer vision. The robot used its onboard camera to detect a colored line, process the image in HSV color space, calculate the position error, and adjust its movement to follow the line.

By tuning HSV thresholds and control parameters such as `Kp`, the robot was able to improve its tracking response and follow the line more reliably. This project provided practical experience in combining computer vision, feedback control, and autonomous mobile robot navigation.


