---
title: Bazinga the Retarded Dog
author: Anna
layout: post
permalink: /bazinga-the-retarded-dog/
enable_retweet_button:
  - 1
custom_retweet_text:
  - 
views:
  - 5
categories:
  - Robot learning
tags:
  - AI
  - Aibo
  - artificial curiosity
  - reinforcement learning
  - robotics
  - Webots
---
Since October last year I have been working on a robot simulation (Sony&#8217;s Aibo) trying to implement artificial curiosity and learning algorithms. Why? Final-year project. But also a very interesting and challenging problem to solve. I am working under supervision of Dr Chrisantha Fernando, at Queen Mary University of London.

<!--more-->
As of now, Bazinga is not the smartest dog out there:



I am now looking into Q-Learning temporal-difference control algorithm to make use of the red ball. The idea is for the Bazinga to be rewarded if it gets closer to the ball. One problem I am currently trying to solve:

The data structure to hold state-action values, e.g. 3-dimensional array where first and second indices are x and y coordinates and third index is a possible action from current state. Considering that Bazinga&#8217;s movement is controlled by 12 joints, this would be quite large array. I could simplify it by replacing joint movement with simple North, South, West, East directions. This seems like oversimplifying though.

Time to read more about reinforcement learning.

P.S. Yes, I am a big fan of the Big Bang Theory.