---
title: How to add a new sensor to Aibo/NAO simulation in Webots
author: Anna
layout: post
permalink: /how-to-add-a-new-sensor-to-aibonao-simulation-in-webots/
enable_retweet_button:
  - 1
custom_retweet_text:
  - How to add a new sensor to Aibo/NAO simulation in Webots
views:
  - 26
categories:
  - Robot learning
tags:
  - Aibo
  - NAO
  - sensor
  - Webots
---
Most of the robot simulations can be modified by adding Nodes directly in Scene Tree in Webots itself. However, to add a new sensor to Aibo (and NAO), we need to modify proto file.
<!--more-->
Example path on OS X (can differ from system to system):
/Applications/Webots/resources/projects/robots/aibo/protos/Aibo_ERS7.proto

As an example, to add a distance sensor we need to paste this into the file:

```
    DistanceSensor {
                   translation 0 0.03 -0.0146
                   rotation 0 0 1 -1.57
                   name "color_sensor"
                   lookupTable [ 0 0 0,
                                 0.1 500 0.1,
                                 0.2 250 0.1,
                                 0.3 50 0.1,
                                 0.37 30 0 ]
                   }
```

More information on DistanceSensor properties can be found [here][1].

 [1]: http://www.cyberbotics.com/reference/section3.21