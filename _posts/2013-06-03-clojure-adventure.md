---
title: Setting up IntelliJ for Clojure
author: Anna
layout: post
permalink: /clojure-adventure/
views:
  - 317
categories:
  - Clojure
  - Tools
tags:
  - clojure
---
I have been interviewing for a summer internship and my interviewers and I talked about Clojure. I am now intrigued by functional programming.

  * <span style="line-height: 1.6;">Parentheses? Not so scary considering they &#8220;replace&#8221; Java&#8217;s (the only language I currently know) curly brackets and multiple commas. Code is surprisingly easier to read.</span>
  * <span style="line-height: 1.6;">Immutable data? A chance to stop wasting time thinking of when and how object&#8217;s state is modified.</span>
  * Being based on JVM allows to use Java&#8217;s libraries

Whether I get this internship or not, I decided to learn Clojure <img src="http://annapawlicka.com/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" /> First step: **setting up IntelliJ.**

  1. <span style="line-height: 15px;">Download and install La Clojure plugin.<br /> </span><span style="line-height: 15px;">Open <b>File/Settings/Plugins</b> from a menu, than click on <b>Browse repositories&#8230;</b> button at the bottom of the Settings window. Type in Clojure in the search window and select <b>La Clojure</b> from plugins list. Right click on the item and select <b>Download and Install</b> command. Restart IntelliJ.</span>
  2. Add Clojure Facet to project modules.  
    Select **File/Project** structure from a menu, then navigate to your module in **Modules** tab, click on a **+** sign and finally select **Clojure**.
  3. To start Clojure REPL for your project select **Tools/Start Clojure Console **from a menu.  
    The REPL will set a classpath for all of your libraries and sources referenced in a current module.
  4. To load a current Clojure file to REPL by a load-file function, select **Tools/Clojure REPL/Load file to REPL**.

It&#8217;s that simple.

P.S. It&#8217;s good to add [leiningen][1] to IntelliJ (easier to get a  
project up and running, build, run tests) but for now I am skipping this step.

 [1]: http://leiningen.org/