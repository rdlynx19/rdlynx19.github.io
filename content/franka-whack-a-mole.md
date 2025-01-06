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
This post is **Under Construction**. Please check back on 18th January, 2024.   
### Demo Video
This project involves making a ROS2 package to enable the Franka Emika Robot Arm to play a game of Whack-a-Mole using MoveIt2 and OpenCV.

This project is a group effort including Ben Benyamin, David Khachatryan, Sairam Umakanth, Haodong Wang, and Pushkar Dave

---
### Hardware Setup

The hardware setup requires a Franka Robot Arm, a Realsense RGBD Camera, an April Tag attached to the robot's base, a Servo Motor controlled Hammer, and a Whack a Mole game set. We built our own Whack a Mole game board to have control over it's speed and get feedback for debugging.
![image](/images/projects/franka-mole/Franka-setup.jpg)

---
### Overview of our Approach

There were **4** major steps involved:
 - Detecting the Colored Buttons a.k.a Moles
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

Cooking...

---

### ROS Nodes
Cooking...

<details>
  <summary><b>comm</b>: handles communication between ROS and the servo motor that controls the hammer's movement</summary>
   - Services: 
  </details>

<details>
    <summary><b>swing</b>: sends motion plan generation and execution requests to the MoveIt2 API via the custom wrapper</summary>
    <p>The swing node acts as an interface between the whack-a-mole game logic and MoveIt2, a motion planning library. It sends requests for motion planning to actuate the hammer and ensures the hammer swings accurately towards the target.</p>
  </details>

<details>
    <summary><b>camera</b>: uses Realsense topics for color detection and broadcasts tf of colors</summary>
    <p>The camera node captures images using a Realsense camera and processes color information. It uses ROS topics to publish color detection results and broadcasts the transform (tf) data for detected colors to the system.</p>
  </details>

<details>
    <summary><b>hint</b>: receives feedback from the whack-a-mole game board to indicate which button has been illuminated (used as a fallback)</summary>
    <p>This node communicates with the Arduino board that powers the physical game board. It detects when a button is illuminated and sends feedback to the system, providing a fallback in case the primary detection method fails.</p>
  </details>

<details>
    <summary><b>game</b>: looks up transforms to find the position of the buttons and sends service requests to actuate the hammer</summary>
    <p>The game node is responsible for determining the position of the illuminated buttons on the game board. It looks up the transform data to calculate the button's location and then sends service requests to actuate the hammer to strike the corresponding button.</p>
</details>

---

### Wrapper for the MoveIt2 API

Cooking...
