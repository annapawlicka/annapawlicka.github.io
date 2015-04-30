---
title: Line following behaviour
author: Anna
layout: post
permalink: /line-following-behaviour/
views:
  - 19
categories:
  - Robot learning
tags:
  - e-puck
  - genetic algorithm
  - line following
  - neural network
---
I&#8217;ve been trying to perfect e-puck&#8217;s line following ability and I think I&#8217;ve succeeded:

<!--more-->

Behaviour was evolved using feedforward neural network and genetic algorithm.<em id="__mceDel"></em>

**Summary of parameters:**  
Inputs: 3 (Floor colour sensors)  
Outputs: 2 (motor actions)  
Number of hidden layers: 1  
Number of neurones in a hidden layer: 12  
Population size: 30  
Selection: Roulette wheel.  
Crossover probability: 50%.  
Mutation probability: 10%.

Quite good behaviour was evolved after just 10 generations. I&#8217;m pretty amazed with how well NN can be trained!