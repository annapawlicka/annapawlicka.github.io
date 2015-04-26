---
title: Q-Learning for Aibo
author: Anna
layout: post
permalink: /a-short-update/
enable_retweet_button:
  - 1
custom_retweet_text:
  - 
views:
  - 7
categories:
  - Robot learning
tags:
  - Aibo
  - artificial curiosity
  - reinforcement learning
  - robotics
  - sensor
  - Webots
---
I&#8217;ve been looking more into motor actions and their effect on Servos.  The initial position is always zero. So each time a new action is applied to the motor (we\_servo\_set_position()), it is interpreted as absolute position. In order to emulate a relative position, I&#8217;ll have to store the last value passed to the method, and then just add that value to the newly computed one. Not sure if this will improve my algorithm as it seems that relative position will eventually result in maxing out the joint position.

I&#8217;m back to **Q-Learning** algorithm. The design is as follows:  
1. **Goal: Approach red ball.** The current distance from the ball minus the last distance reading determines the reward (greater decrease = greater reward). So if the current distance reading is 56 and the previous distance reading was 53, it receives a reward of +3.  
2. **Goal: Avoid Obstacles.** If one of the bumpers is pressed, the reward is -2.  
3. **Goal: Avoid Staying Still.** If the distance reading hasn&#8217;t changed in the last five steps, it receives a negative reward of -2. Presumably, if Aibo is receiving identical distance readings for five or more steps in a row, it is hung up or not moving.So how will the actual Q-Values be calculated? Basically we just need an equation that increases the Q-Value when a reward is positive, decreases the value when it is negative, and holds the value at equilibrium when the Q-Values are optimal. The equation is as follows:

Q(a,i)fl Q(a,i) + ß(R(i) + Q(a<sub>1</sub>,j) &#8211; Q(a,i))

where the following is true:

Q — a table of Q-values  
a — previous action  
i &#8211; previous state  
j &#8211; the new state that resulted from the previous action  
a<sub>1</sub> — the action that will produce the maximum Q value  
ß — the learning rate (between 0 and 1)  
R — the reward function

This calculation must occur *after* an action has taken place, so the robot can determine how successful the action was (hence, why *previous action* and *previous state* are used).

In order to implement this algorithm, all movement by the robot must be segregated into *steps*. Each step consists of reading the percepts, choosing an action, and evaluating how well the action performed. All Q-values will initially be equal to zero for the first step, but during the next step (when the algorithm is invoked), it will set a Q-Value for Q(a,i) that will be a product of the reward it received for the last action. So as the robot moves along, the Q-values are calculated repeatedly, gradually becoming more refined (and more accurate).

I am going to try this out now &#8211; but with very simplified actions (joint angles will be replaced by movements NORTH, SOUTH, etc.).