---
title: "Franka Plays Whack a Mole"
date: 2024-12-12
draft: false
author: "Pushkar Dave"
tags:
  - Python
  - ROS2
  - MoveIt2
  - OpenCV
image: 
description: ""
toc: true
mathjax: true
repoName: franka-whack-a-mole
---

### Demo Video
This project involves making a ROS2 package to make the Franka Emika Robot Arm play a game of Whack-a-Mole using MoveIt2 and OpenCV.

This project is a group effort including Ben Benyamin, David Khachatryan, Sairam Umakanth, Haodong Wang, and Pushkar Dave

---
### Hardware Setup

The hardware setup requires a Franka Robot Arm, a Realsense RGBD Camera, an April Tag attached to the robot's base, a Servo Motor controlled Hammer, and a Whack a Mole game set. We built our own Whack a Mole game board to have control over it's speed and get feedback during debugging.
![image](/images/projects/franka-mole/Franka-setup.jpg)

---
### Overview of our Approach

There were **4** major steps involved:
 - Detecting the Colored Buttons aka Moles
 - Getting the position of the Moles in the Robot's Frame
 - Generating a Motion Plan from the Current Robot State to the Target Mole
 - Actuating the Hammer to Swing and Hit the Mole

![image](/images/projects/franka-mole/NewBlock.png)

---
### Color Detection
Cooking...

---
### April Tags and Transforms

To detect the pose of the moles in the robot's frame, we use a single april tag attached at the base of the robot. Using the `april-tag-node` in ROS2, we published a transform from the camera frame to the april tag located at the base of the robot. 

---


