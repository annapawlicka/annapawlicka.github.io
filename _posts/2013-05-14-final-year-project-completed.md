---
title: Final year project completed
author: Anna
layout: post
permalink: /final-year-project-completed/
views:
  - 26
categories:
  - Robot learning
---
It&#8217;s been a long road, but the project is finally completed: I submitted report and survived my viva <img src="http://annapawlicka.com/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" />

<!--more-->

To sum up:

  * <span style="line-height: 1.6;">Project was part of a study on cognitive architecture that could be implemented in a robot. The aim was to implement algorithms that are based on Darwinian Neurodynamics, to perform experiments and identify future improvements.</span>
  * Two algorithm designs were implemented: co-evolution of Actors and Predictors, and evolution of Solutions (Artificial Feedforward Neural Network) that learn how to play multiple games. Experiments on both algorithms were performed using Webots robot simulator.
  * The former algorithm was implemented on a simulation of a robotic dog, Aibo, the latter using simulation of a robot with two differential wheels, E-puck.
  * The results of experiments on the robotic dog led to discarding both the simulation and the algorithm. The linear model of the algorithm was not suitable to predict non- linear events. Additionally, the complexity of evolving locomotion was a decisive factor in replacing the simulation with a simpler model.
  * Experiments on E-puck conducted using single Game resulted in the targeted behaviour. Implementation of multiple games resulted in mediocre Solutions and led to modification of the algorithm to use Multi-Objective Optimisation.
  * Further improvements that have been identified include modification of Games to increase their complexity and make them evolvable.
  * [GitHub repo][1].

Now my focus in on exams, but I will definitely continue working on this cute little yoghurt pot during summer. So many possibilities can be explored!

&nbsp;

 [1]: https://github.com/apawlicka/darwinian-epuck