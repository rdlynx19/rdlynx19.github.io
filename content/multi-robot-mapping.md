---
title: "Collaborative Mapping Using a Quadrotor and Quadruped"
date: 2025-03-03
draft: false
author: "Pushkar Dave"
tags:
  - C++
  - ROS2
  - Mapping
  - Localization
image: 
description: ""
toc: true
mathjax: true
repoName: multi-robot-mapping
---
## Overview

The project focuses on developing a collaborative mapping system using a Quadrotor (QAV 250) and Quadruped (Unitree Go 1) to efficiently explore and map cluttered environments.
 
The motivation behind this project is rooted in the idea that a combination of a quadrotor and a quadruped could complement each other’s capabilities in scenarios where the environment is completely unknown. The quadrotor’s ability to quickly cover large areas and the quadruped’s capability to navigate complex terrains and provide detailed mapping make this system suitable for applications like search and rescue, industrial inspections, or exploration in hazardous environments.
{{<youtube lgYT0VuTbq0>}}

The QAV 250 is equipped with an Oak RGB-Depth Camera, and Raspberry Pi 5 to autonomously fly around the environment and generate a 3D map using RTABMap package. This map is then transmitted to the Unitree Go 1 (equipped with the same setup), which localizes itself withing the map and refines it by adding finer details through further exploration. The Unitree Go 1 explores the environment through manual control, while ongoing work focuses on deploying frontier exploration for fully autonomous navigation and mapping.

## System Setup

