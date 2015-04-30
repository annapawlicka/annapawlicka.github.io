---
title: Standing position of Bazinga as a new fitness function.
author: Anna
layout: post
permalink: /standing-position-of-bazinga-as-a-new-fitness-function/
enable_retweet_button:
  - 1
custom_retweet_text:
  - 
views:
  - 1
categories:
  - Robot learning
tags:
  - AI
  - Aibo
  - artificial curiosity
  - reinforcement learning
  - robotics
  - sensor
  - Webots
---
It turns out that the infra red sensor does not work as I expected. First of all, it interprets lighter shades of various colours as red (e.g. yellow!). I&#8217;ve tried to modify the lookup table, but could not find the correct settings. Second, it does not work in fast mode (vital for evolution!). To sum up, it&#8217;s not a reliable sensor to be used in fitness function. I will definitely implement the red ball seeking behaviour, but instead of using the infra red sesor, I&#8217;ll do some simple camera image processing. However, before I jump into that, I&#8217;d like to make the dog move and not jerk in a random manner!

<!--more-->
I&#8217;ve been thinking of how I could make Bazinga to learn some sensible actions, and I decided to choose its chest distance sensor. It is located 30 degrees towards the ground, facing forward. When Bazinga lies on its belly, the reading is 100, when it lies on its back it&#8217;s 900. So the ideal value would be around 300-400 probably, and it would lead to a standing position.

I&#8217;ve made some initial trials, but it seems that the joint angles are being set into awkward positions, twisting legs towards the back. I still need to look into the frequency with which population should be evolved, and also decide how to select what motors should be activated given 12 sensorimotor inputs (currently these are 4 random motors).

I will carry on tomorrow..