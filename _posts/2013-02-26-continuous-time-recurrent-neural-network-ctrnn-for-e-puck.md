---
title: Continuous Time Recurrent Neural Network (CTRNN) for e-puck
author: Anna
layout: post
permalink: /continuous-time-recurrent-neural-network-ctrnn-for-e-puck/
enable_retweet_button:
  - 1
custom_retweet_text:
  - 
views:
  - 197
categories:
  - Robot learning
tags:
  - CTRNN
  - e-puck
  - robotics
  - Webots
---
After performing some experiments with Q-Learning algorithm, reading tons of research papers and finally realizing that evolving walking in Aibo is a research project on its own, I decided to change the simulation to e-puck.
<!--more-->
e-puck is a differential wheels robot, kindly nicknamed &#8220;yoghurt pot&#8221; by my friends:

<span style="line-height: 1.6;">Why e-puck? Only two wheels to control (as opposed to 16 joints in Aibo). As a bonus, once the algorithm is completed I&#8217;ll have a chance to test it on a real robot!</span>

<span style="line-height: 1.6;">The algorithm I&#8217;m implementing is Continuous Time Recurrent Neural Network (CTRNN). It&#8217;s just a first step. Second step will be to evolve two populations: agents and games. Games will represent different fitness functions, agents &#8211; neural networks. I&#8217;m back to evolutionary computation.</span>