---
title: A not very successful attempt at red ball fitness function.
author: Anna
layout: post
permalink: /a-not-very-successful-attempt-at-red-ball-fitness-function/
enable_retweet_button:
  - 1
custom_retweet_text:
  - 
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
I decided to try a very simple genetic algorithm, with my own methods to copy and mutate individuals, and a very basic fitness function that, in theory, should try to minimize the distance between Bazinga and the red ball.

Results:



Problem: Bazinga has no idea how to walk. It gets stuck when facing the wall. Its movements resemble jerks.

Possible solution 1: Input comes from 12 joints. Motor actions are applied to 12 joints as well. I will limit the number of outputs to e.g. 4.

Possible solution 2: Four legged animals perform synchronized movements. Should I implement that? They are also born with reflexes. Maybe it would be smarter to hardcode some basic reflexes?

Possible solution 3: The fitness function could measure the speed of getting from one place to another. This could theoretically evolve the most efficient way of moving. This may not resemble typical walking at first, but maybe eventually it would lead to it, e.g. lizards vs mice (their efficiency of locomotion is affected by the setting of their limbs).