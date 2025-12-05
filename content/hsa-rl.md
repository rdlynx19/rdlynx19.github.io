---
title: "Emergent Locomotion in a Handed Shearing Auxetic (HSA) Actuated Robot"
date: 2025-12-04
draft: false
author: "Pushkar Dave"
tags:
  - Python
  - Reinforcement Learning
  - MuJoCo
  - Simulation
  - Modeling
image: 
description: ""
toc: true
mathjax: true
repoName: hsa
---
## Overview

This project focused on modeling, simulating, and developing locomotion gaits for a soft-robotic module based on **Handed Shearing Auxetic (HSA)** actuators. The primary goal was to reproduce the physical module’s behavior in simulation, and then apply learning-based methods to discover novel and unintuitive gaits. The project consisted of three phases:

1. **Modeling a physics-informed approximation** of the HSA actuators to capture their deformation and actuation behavior in simulation.
2. **Replicating existing gaits** observed on the physical robot to validate that the simulated system reflected real-world dynamics.
3. **Training the robot** across various terrains, using **reinforcement learning** - enabling the emergence of new and unintuitive locomotion strategies.

---

## System Modeling

### Handed Shearing Auxetic (HSA)
Handed Shearing Auxetic (HSA) actuators are soft, flexible structures that change shape in unusual ways when they are twisted or compressed. They can stretch, twist, and deform smoothly based on their internal pattern.

Each structure can be either left-handed or right-handed, which means it naturally twists in a different direction when it moves. By combining these opposite twists in a unit, we can create a variety of movement patterns and behaviors. 

<img src="/images/projects/hsa-rl/handedness.png" alt="Handedness View" style="width: 100%; height: auto;"/>


The <a href="https://sites.northwestern.edu/roboticmatterlab/" target="_blank">Robotic Matter Lab</a> at Northwestern University has been developing a robot out of this structural arrangement. Specifically, four HSA modules with pairwise opposite handedness are combined, with continous rotation servo motors attached at each end. The resulting robot is shown in the images below.


<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/hsa-rl/RealHSAModule.png" alt="Rendered View" style="width: 48%; height: auto;"/>
  <img src="/images/projects/hsa-rl/ActualHSA.png" alt="Physical View" style="width: 48%; height: auto;"/>
</div>
<br>

### Simulation Model
![block diagram](/images/projects/hsa-rl/HSAModel.png)



Both the quadrotor and the quadruped are equipped with Raspberry Pi 5, which runs the control and camera nodes. The camera data is streamed over a Data Distribution Service (DDS) setup to a remote Linux laptop. On the laptop, I use the RTABMap package to process the data, running its odometry and mapping nodes to build the 3D map in real time. This setup helps extend the robots' operational time by offloading computation to the laptop, reducing the load on their onboard LiPo batteries.

---

## QAV 250
The quadrotor setup consists of the following components:
- Holybro QAV 250 Quadrotor Frame
- PixHawk 6C Mini AutoPilot
- Raspberry Pi 5 with Ubuntu Linux 24.04 and ROS2 Jazzy
- Luxonis OakD-Lite RGB Depth Camera
- 4S 2000mAh Lipo Battery

<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/DroneTop.JPEG" alt="Top View" style="width: 48%; height: auto;"/>
  <img src="/images/projects/multi-robot/DroneBottom.JPEG" alt="Bottom View" style="width: 48%; height: auto;"/>
</div>
<br>

The LiPo battery powered all components of the quadrotor, including the motors, autopilot, and the Raspberry Pi 5. Since the Raspberry Pi 5 requires a stable 5V, 5A power supply, a buck converter was used to step down the voltage from the 4S LiPo battery (16.8V to 14.8V) to the required 5V.

---

## Unitree Go 1

The quadruped setup consists of the following components:
- Unitree Go 1
- Raspberry Pi 5 with Ubuntu Linux 24.04 and ROS2 Jazzy
- Luxonix OakD-Lite RGB Depth Camera
- 4S 1300mAh Lipo Battery


<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/dogSetupb.JPEG" alt="Top View" style="width: 48%; height: auto;"/>
  <img src="/images/projects/multi-robot/DogMount.png" alt="Bottom View" style="width: 48%; height: auto;"/>
</div>

During the project, I designed and 3D-printed a bunch of custom parts to make everything fit together just right. One of the key pieces I made was a mount for the Oak-D Lite camera and the Raspberry Pi 5, specifically for the Unitree Go1. It took a few iterations to get it perfect, but the final version kept everything secure and stable, even while the robot was moving around.

---

## Autonomous Drone Exploration

### OptiTrack Motion Capture
To fly a drone autonomously indoors, you need a reliable position estimate, either from onboard sensors or an external system like a motion capture setup. Since I was working with a 250mm quadrotor frame, I had to carefully balance space and weight constraints. Onboard visual-inertial odometry (VIO) was an option, but running a VIO framework on the Raspberry Pi 5 would have consumed most of its compute power, leaving little room for other critical tasks like communication nodes.

After weighing the options, I decided to use the OptiTrack motion capture system for position estimation during autonomous flight. It’s worth noting that the motion capture system was only used to enable the drone’s autonomous flight—no odometry data from the drone was used during the mapping process. All maps were created using RTABMap, relying solely on RGB-D odometry from the onboard camera.

<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/optiTrack.png" alt="Top View" style="width: 80%; height: auto;"/>
</div>

---

### Offboard Control

To enable the drone to explore the environment autonomously, I developed a custom ROS2 C++ package for offboard control. This package runs onboard the Raspberry Pi 5 and communicates with the PixHawk 6C Mini flight controller via serial communication using the uXRCE-DDS client. The package includes custom services to handle essential drone operations, such as arming, disarming, takeoff, and landing.

Additionally, I implemented a function to generate polygonal trajectories, allowing the drone to systematically explore the environment and map its features.

<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/shrunkPolyTraj.gif" alt="Top View" style="width: 80%; height: auto;"/>
</div>

---

## RTABMap 

The RTABMap package in ROS2 was used to process the RGB and Depth data from the Oak-D Lite camera. Using the IMU data from the camera, loop closure and graph optimization, the map remained consistent and accurate, even as the drone and quadruped explored new areas. RTABMap also provided the occupancy grid data that powered the frontier exploration algorithm, making it critical for both mapping and localization.

### RGBD Odometry 
RTABMap estimates odometry by extracting features using an algorithm like Speeded-UP Robust Feature (SURF). By matching features between consecutive frames and incorporating IMU data from the camera, it determines the camera's movement. Using these matched features along with depth information, the package computes the relative transformation between frames. This odometry output is then used to track the drone's position and orientation as it navigates through the map.

An example of RTABMap tracking features between images captured at different times is shown here.

<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/rtabmapViz.png" alt="Top View" style="width: 80%; height: auto;"/>
</div>

### 3D Mapping
Once odometry is established, RTABMap constructs a point cloud of the environment using RGB-D data. As new frames are processed, their point clouds are aligned with the existing map based on odometry data. To correct for accumulated errors, loop closure is used to optimize the entire map and mitigate drift. The map is updated incrementally as new data is processed, allowing RTAB-Map to efficiently handle large environments by storing and updating only the most relevant parts in memory.

<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/3DPointcloud.png" alt="Top View" style="width: 80%; height: auto;"/>
</div>

---
## Frontier Exploration for Unitree Go 1

To recall, the system works in two phases: first, the QAV 250 drone explores the environment by following a polygonal trajectory, creating a sparse 3D map using RTABMap. This map is then passed to the Unitree Go1 quadruped, which localizes itself and refines the map by adding more details. Currently, the quadruped’s exploration is manual (it is teleoperated using a joystick), but I am working towards making the exploration autonomous.

To achieve this, I developed a frontier exploration algorithm in ROS2 C++. It analyzes the map from RTABMap, identifies unexplored areas (frontiers), and sends the quadruped to explore them. I tested the algorithm in simulation by moving the Oak-D Lite camera around manually and visualizing the frontier goals in RViz. 
<div style="display: flex; justify-content: space-between;">
  <img src="/images/projects/multi-robot/extractFrontier.gif" alt="Top View" style="width: 80%; height: auto;"/>
</div>

---
## Future Work

With more time, I would like to add the following functionalities to this project:
- Deploy autonomous exploration on the Unitree Go1 using the Unitree ROS2 SDK.
- Enable simultaneous exploration, where the quadrotor and quadruped work together in real time for faster and more efficient mapping.

---

## Acknowledgements

This project is part of my **Master of Science in Robotics** at **Northwestern University**. 
Thank you to Prof. Paul Umbanhowar for his guidance and support throughout the project, his feedback was incredibly helpful. Special thanks to Drew Curtis for helping me with the OptiTrack system, which made the autonomous drone flight possible.

