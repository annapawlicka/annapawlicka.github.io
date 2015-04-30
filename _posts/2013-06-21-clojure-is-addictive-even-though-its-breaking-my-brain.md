---
title: 'Clojure is addictive (even though it&#8217;s breaking my brain)'
author: Anna
layout: post
permalink: /clojure-is-addictive-even-though-its-breaking-my-brain/
views:
  - 79
categories:
  - Clojure
tags:
  - clojure
---
I'm currently reading Clojure in Action, and going through [4clojure][1] exercises. I have also set up a [github repo][2] for my solutions. Feel free to suggest better solutions!

<!--more-->
I'm getting used to the syntax (parentheses are not scary at all!). And I'm slowly (but surely?) changing the way I think of data structures and functions.

I make mistakes. Some are embarrassing.  
When asked to write a function that reverses a sequence (but not using `reverse` or `rseq`), my initial solution was this:

```clojure
(fn [coll]
  (loop [r (rest coll) result (conj () (first coll))]
    (if (empty? r)
      result
     (recur (rest r) (conj result (first r))))))
```

I was not really using what Clojure has to offer. It can be done as a one-liner:

  
```clojure
#(reduce conj () %)
```

But I'll get there!


 [1]: http://www.4clojure.com/
 [2]: https://github.com/apawlicka/4clojure-solutions