---
title: 'Evolution of neural network: separate trials for each individual?'
author: Anna
layout: post
permalink: /evolution-of-neural-network-separate-trials-for-each-individual/
custom_retweet_text:
  - 
views:
  - 12
categories:
  - Robot learning
tags:
  - CTRNN
  - robotics
---
A few thoughts about the evolution. <span style="line-height: 1.6;">In a population of 50 neural networks, how do I select which one will decide on next motor action?</span>

<!--more-->

  * <em id="__mceDel"><span style="line-height: 1.6;">Initially, all individuals have fitness zeroed. </span></em>
  * <em id="__mceDel"><span style="line-height: 1.6;">Then each individual will be given a certain number of trials. </span></em>
  * <em id="__mceDel"><span style="line-height: 1.6;">After that its overall fitness will be calculated, and next individual will be tested. </span></em>
  * <em id="__mceDel"><span style="line-height: 1.6;">When all individuals are assessed, a new generation will be created (by using tournament selection, crossover and mutation).</span></em>

So basically, the environment will be reset for each individual, and this individual will be the only one to decide on action in this trial.

Question: If I reset the environment for each trial, will it not affect the learning?