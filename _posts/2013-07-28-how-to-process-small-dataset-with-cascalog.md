---
title: How to process (small) dataset with Cascalog
author: Anna
layout: post
permalink: /how-to-process-small-dataset-with-cascalog/
views:
  - 1861
categories:
  - Cascalog
  - Clojure
tags:
  - Cascalog
  - clojure
  - Emacs
  - Hadoop
---
My learning at university involved processing Twitter dataset using Hadoop cluster (as part of a very useful module called High Performance Computing). All done using pure Java. These are not good memories. Jobs failing, jobs queuing, cluster experiencing downtime, students panicking, staff complaining. Not to mention the need to write my own Mapper, Reducer, Combiner and a job configuration. And the job was really tiny! Oh, and the need to create a new jar and copy it to hdfs each time I changed the code. If only I could just write a simple query and run it the way SQL is run..

One word:* Cascalog.*

<!--more-->

If the job does not take longer than 20 minutes to run locally, then you really don&#8217;t need a Hadoop cluster for that. With Cascalog you can write your job in a form of a query, and you can write Clojure functions to do, well, whatever you want with your data. Read below to find out how to set up your environment to work with Clojure Cascalog (there is a pure Java Cascalog too, [JCascalog][1], but NO.)

**Hadoop installation**

OS X: For the most useful instructions on how to get Hadoop on your Mac, using homebrew, head to Denny Lee&#8217;s [article][2].  
Other operating systems: [Apache&#8217;s Wiki][3] seems like a good source.

**Clojure installation**

OS X: You can do it quickly and pain-free using homebrew (and if you don&#8217;t have homebrew, shame on you!). Instructions [here][4].

Pay attention to Step 3, where you learn how to set up a Clojure project. Your project.clj is a one-stop shop for downloading all dependencies, so this is where you&#8217;ll add your hadoop and cascalog jars.

<pre class="brush:java">(defproject project-name "0.1.1-SNAPSHOT"
  :description "Description"
  :url "https://www.url.com"

  :dependencies [[org.clojure/clojure "1.5.1"]
                 [cascalog "1.10.1"]
                 [org.clojure/math.numeric-tower "0.0.2"]]
  :profiles {:dev {:dependencies [[midje "1.5.1"
                                   :exclusions [org.apache.httpcomponents/httpcore
                                                commons-io]]
                                  [org.apache.hadoop/hadoop-core "1.0.4"
                                   :exclusions [org.slf4j/slf4j-api
                                                commons-logging
                                                commons-codec
                                                org.slf4j/slf4j-log4j12
                                                log4j]]]}
             :provided {:dependencies [[org.apache.hadoop/hadoop-core "0.20.2-dev"]]}}
  :main your-main
  :uberjar-name "your-jar.jar"
  :exclusions [org.apache.hadoop/hadoop-core
               org.clojure/clojure
               midje])</pre>

&nbsp;

**Emacs**

I&#8217;m new to Emacs, it&#8217;s been a steep learning curve, but an enjoyable one. Emacs is not as memory hungry as your typical IDE. And using nrepl in Emacs proved to be easier than using it in IntelliJ (haven&#8217;t tried other IDEs though).

To write and run your code, get yourself [Emacs-live][5]. It&#8217;s a complete setup so everything you need is already there. If you haven&#8217;t used Emacs before it&#8217;s best if you go through the built-in tutorial. You can open it from inside Emacs Live using *M-h t* (should be *Alt+h* followed by *t*). In Emacs the two commonly-used modifier keys are <Control> (usually labeled <Ctrl> and referred to as C), and <Meta> (usually labeled <Alt> and referred to as M).<a href="http://www.gnu.org/software/emacs/manual/html_node/emacs/User-Input.html#fn-1" rel="footnote" name="fnd-1"><sup><br /> </sup></a>

Open your project.clj (or any other clojure file) in Emacs, and connect to it nrepl by running *M-x nrepl-jack-in*. This will load all your dependencies and set up your classpath.  
For more commands, use [Emacs cheatsheet][6].

**Write queries**

Let&#8217;s say we have a series of CSV files, each looking like this: *store\_id,store\_name,city,sales_total*  
and containing sales from a single month. We want to go through all the files and get the total sales (12 months worth) from all stores per each city.

First, let&#8217;s create our own namespace, and import Cascalog and some other namespaces:  
&#8211; cascalog.ops to use sum function  
&#8211; cascalog.more-taps to parse delimited files, e.g. CSV

<pre class="brush:java">(ns my.project.sales
  (:require [cascalog.api :refer :all]
            [cascalog.ops :as ops]
            [cascalog.more-taps :refer [hfs-delimited]]))</pre>

Next, let&#8217;s create some helper functions. We&#8217;ll be dealing with numbers so it&#8217;s important to check if the value in a given column is actually a number, and then parse it:

<pre class="brush:java">(defn numbers-as-strings? [& strings]
  (every? #(re-find #"^-?\d+(?:\.\d+)?$" %) strings))

(defn parse-double [txt]
  (Double/parseDouble txt))</pre>

Now we can construct our query (and write it as a function of course):

<pre class="brush:java">(defn total-sales-per-city [input]
  (&lt;- [?city ?total]
      (input :&gt; ?store-id ?store-name ?city ?sales-string)

      (numbers-as-strings? ?sales-string)
      (parse-double ?sales-string :&gt; ?sales)

      (ops/sum ?sales :&gt; ?total)))</pre>

Query takes a single line from the file as an input, bounds each coma-separated value to a variable (preceded with ?), checks and parses numbers, and then uses sum function to add up all sales. Since we declared *?city* as a result variable of the query, Cascalog will partition the records by *?city* and apply the *ops/sum* aggregator within each partition. Quite nifty, isn&#8217;t it?

Last step is to use the above function:

<pre class="brush:java">#_ (let [data-in "./input/sales/"
         data-out "./output/total-sales-per-city/"]
     (?- (hfs-delimited data-out :sinkmode :replace :delimiter ",")
         (total-sales-per-city
          (hfs-delimited data-in :delimiter ","))))</pre>

*?-* executes the query and emits the results to the specified tap. In the example above, I assigned input path to a variable data-in, and output path to data-out. Taps I&#8217;m using are both *hfs-delimited*, as I&#8217;m reading from a CSV, but I also want to write to a CSV. In order to overwrite the resulting file (if we don&#8217;t want to delete the file each time manually) we can specify *:sinkmode* as *:replace*.

To run:  
1. Set the namespace to the current buffer C-c M-n  
<span style="line-height: 1.6;">2. Compile: C-c C-k<br /> </span><span style="line-height: 1.6;">3. Move the cursor over to the query and execute: C-M-x</span>

That&#8217;s it. It&#8217;s really that simple. For more examples, head to Nathan&#8217;s [article][7].

Enjoy writing queries, because Cascalog is fun!

 [1]: https://github.com/nathanmarz/cascalog/wiki/JCascalog
 [2]: http://dennyglee.com/2012/05/08/installing-hadoop-on-osx-lion-10-7/
 [3]: http://wiki.apache.org/hadoop/GettingStartedWithHadoop
 [4]: https://gist.github.com/haksior/2397208
 [5]: https://github.com/overtone/emacs-live
 [6]: http://www.cheatography.com/cmdrdats/cheat-sheets/my-emacs-cheat-sheet/
 [7]: http://nathanmarz.com/blog/introducing-cascalog-a-clojure-based-query-language-for-hado.html