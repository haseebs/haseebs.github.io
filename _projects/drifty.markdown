---
layout: default
title:  "Drifty"
date:   2015-12-13 02:54:25 +0500
categories: project
description: Cross-platform 3D game in which the player has to maneuver a slippery vehicle through an endless map of obstacles. It was designed to run smoothly on low-end devices and features an online leaderboard.
---
# **Drifty**
## **Overview**
**Semester:** 2nd

**Project Date:** February 2016

**Project Duration:** 2 months

**Languages/Frameworks:** C#, Unity Engine, Blender

**Android Download:** [Link](/assets/downloads/drifty/drifty-android.apk)

**Windows Download:** [Link](/assets/downloads/drifty/drifty-windows-desktop.zip)

**Short Description:** Cross-platform 3D game in which the player has to maneuver a slippery vehicle through an endless map of obstacles. It was designed to run smoothly on low-end devices and features an online leaderboard.

## **Overview**
This game features a slippery vehicle which has to be maneuvered safely
through obstacles. The obstacles are generated from custom obstacle maps
whose orders are randomized at each step. All the resources are loaded
at the start of the game using Object Pooler pattern to save
computational time during the gameplay.

One of the main objectives during the design of this game was that the
game should run smoothly even on the low-end devices. So, for this
reason, all the models that I built for this game were designed in a way
that they use lower number of polygons. A texture map was built which
had a grid of solid colors so that all the different models can use a
single texture file to color themselves. This greatly reduced the memory
load. Further, there are options in the settings which allow the player to
turn off the shadows and smoke, which result in a massive
performance improvement.

## **Class Diagram**
![Class Diagram](/assets/media/drifty/class_diagram.png)

## **Screenshots**
### **Main Menu**
![Main Menu](/assets/media/drifty/main_screen.png)

### **Car Selection**
![Car Selection](/assets/media/drifty/car_selection.png)

### **Gameplay**
![Gameplay](/assets/media/drifty/gameplay.png)

### **Car Crash**
![Car Explosion](/assets/media/drifty/crash.png)

## **Future Plans**
Future plan involves adding the missing features in the game and
application of Reinforcement Learning for the generation of trickier
obstacle maps after the agent learns to maneuver the vehicle correctly
in premade maps.
