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
I've been looking for an easy way to format code snippets for my Keynote presentation and everything seemed quite awkward to use (especially taking screenshots!). I could just add a link to the example and make it open a local webpage, but I&#8217;d rather not do that for very short snippets. After some quick research I&#8217;ve settled with <a title="highlight" href="http://www.andre-simon.de/doku/highlight/en/highlight.html" target="_blank">highlight</a>.

You can install it with `homebrew:`

```
brew install highlight
```

I put my snippets into a file (snippets.clj in the example below), and then run this to copy the formatted snippet to clipboard:

```
pbpaste | highlight --syntax=clojure -O rtf snippets.clj | pbcopy
```

<p style="color: #000000;">
  <code>-O rtf </code>specifies format of the output file (which is rtf for Keynote). Highlight supports <a title="lots" href="http://www.andre-simon.de/doku/highlight/en/langs.php" target="_blank">lots</a> of languages and customising possibilities are endless. The end effect looks like this:
</p><figure id="attachment_271" style="width: 580px;" class="wp-caption alignnone">

<img class="wp-image-271 size-large" src="/images/keynote-code.jpg" alt="Sample slide with code snippet" width="580" height="327" />