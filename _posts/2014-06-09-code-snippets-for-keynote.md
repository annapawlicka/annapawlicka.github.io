---
title: Code snippets for Keynote
author: Anna
layout: post
permalink: /code-snippets-for-keynote/
views:
  - 1276
categories:
  - Clojure
  - Discovered
  - JavaScript
  - Tools
---
I&#8217;ve been looking for an easy way to format code snippets for my Keynote presentation and everything seemed quite awkward to use (especially taking screenshots!). I could just add a link to the example and make it open a local webpage, but I&#8217;d rather not do that for very short snippets. After some quick research I&#8217;ve settled with <a title="highlight" href="http://www.andre-simon.de/doku/highlight/en/highlight.html" target="_blank">highlight</a>.

You can install it with homebrew:`<br />
`

<pre style="color: #000000;"><span class="n">brew</span> <span class="n">install</span> <span class="n">highlight</span></pre>

I put my snippets into a file (snippets.clj in the example below), and then run this to copy the formatted snippet to clipboard:`<br />
`

<pre style="color: #000000;"><span class="n">pbpaste</span> <span class="o" style="font-weight: bold; color: #000000;">|</span> <span class="n">highlight</span> <span class="o" style="font-weight: bold; color: #000000;">--</span><span class="n">syntax</span><span class="o" style="font-weight: bold; color: #000000;">=clojure</span> <span class="o" style="font-weight: bold; color: #000000;">-</span><span class="n">O</span> <span class="n">rtf</span> snippets.clj <span class="o" style="font-weight: bold; color: #000000;">|</span> <span class="n">pbcopy</span></pre>

<p style="color: #000000;">
  <code>-O rtf </code>specifies format of the output file (which is rtf for Keynote). Highlight supports <a title="lots" href="http://www.andre-simon.de/doku/highlight/en/langs.php" target="_blank">lots</a> of languages and customising possibilities are endless. The end effect looks like this:
</p><figure id="attachment_271" style="width: 580px;" class="wp-caption alignnone">

[<img class="wp-image-271 size-large" src="http://annapawlicka.com/wp-content/uploads/2014/06/Untitled-940x531.jpg" alt="Sample slide with code snippet" width="580" height="327" />][1]<figcaption class="wp-caption-text">Sample slide with code snippet formatted using highlight</figcaption></figure>

 [1]: http://annapawlicka.com/wp-content/uploads/2014/06/Untitled.jpg