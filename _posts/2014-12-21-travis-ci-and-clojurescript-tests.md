---
title: Travis CI and ClojureScript tests
author: Anna
layout: post
permalink: /travis-ci-and-clojurescript-tests/
views:
  - 606
categories:
  - Clojure
  - ClojureScript
tags:
  - cljs.test
  - clojure
  - ClojureScript
---
I have something to confess: I haven&#8217;t written any ClojureScript tests until this very morning. There were various reasons for that and often I would copy whatever was possible to Clojure namespace and test it there using `clojure.test`. So when a new ClojureScript with a port of the clojure.test namespace &#8211; `cljs.test` &#8211; was released, I could no longer ignore it.

I wrote some tests, I googled for information on how to actually run them, and I hit the wall. Most of the posts were outdated and if it weren&#8217;t for <a href="https://twitter.com/apkeedle" target="_blank">Andrew Keedle</a>&#8216;s post on <a href="http://keeds.github.io/clojurescript/2014/12/19/cljs-test.html" target="_blank">basic cljs.test setup</a>, I&#8217;d probably be still banging my head against the wall.

I like open source projects, and I like Travis CI (which is free for open source projects), so the next step was quite obvious:

##### Set up ClojureScript tests with Travis CI

1. Add `.travis.yml`

    ```clojure
    lein: lein2
    script: lein2 cljsbuild test
    ```

2. Add test command to your `project.clj`

    ```clojure
    :test-commands {"test" ["phantomjs" "phantom/unit-test.js" "phantom/unit-test.html"]}
    ```

If you followed Andrew&#8217;s post you should have `phantom` directory in your project already, and `unit-test.js` and `unit-test.html` within it.

With this setup you can now either continuously test your project by running `lein cljsbuild auto test` (tests will be run whenever you save changes to your project) or you can run your tests manually by executing `lein cljsbuild test`. The latter option will be invoked by Travis.

For a real life, but tiny, example you can go to my <a href="https://github.com/annapawlicka/is-it-time" target="_blank">is-it-time</a> repository.

I hope this is of help!