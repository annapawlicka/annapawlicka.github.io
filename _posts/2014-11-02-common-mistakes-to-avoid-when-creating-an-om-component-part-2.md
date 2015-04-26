---
title: Common mistakes to avoid when creating an Om component. Part 2.
author: Anna
layout: post
permalink: /common-mistakes-to-avoid-when-creating-an-om-component-part-2/
views:
  - 1427
categories:
  - Clojure
  - ClojureScript
tags:
  - clojure
  - ClojureScript
---
It's been a while since the last post. More mistakes have been made, lessons have been learned, so here's a handful:

  1. <p class="p1">
      <strong><span class="s1">Combining cursors</span></strong>
    </p>
    
    Make sure you&#8217;re updating the cursor and not the map that combines them.  
    Let&#8217;s say you want to create a component that has a view of bands and titles of films. You don&#8217;t want to pass whole of the app state, you just want to pass :albums and :titles. You combine them like this:
    
    <pre class="brush: clojure; title: ; notranslate" title="">(def app-state {:music {:artists []
                        :bands {:faved []}
                        :albums {:faved []}}
                :films {:directors []
                        :titles {:faved []}}})

(om/build select-favourites {:bands  (-&gt; cursor :music :bands)
                             :titles (-&gt; cursor :films :titles)})

</pre>
    
    Inside of you select-favourites component you might assume that cursor is an actual cursor. It&#8217;s not, it&#8217;s just a map containing cursors. You can&#8217;t do this:
    
    <pre class="brush: clojure; title: ; notranslate" title="">(defn select-favourites [cursor owner]
  (om/component
   ...
   (om/transact! cursor [:bands :faved] #(conj % some-band))))
</pre>
    
    You can&#8217;t update the map. You need to get the cursor out of the map:
    
    <pre class="brush: clojure; title: ; notranslate" title="">(defn select-favourites [{:keys [bands titles]} owner]
  (om/component
   ...
   (om/transact! bands :faved #(conj % some-band))))
</pre>

  2. <p class="p1">
      <span class="s1"><strong>Method signature and protocol implementation</strong></span>
    </p>
    
    Sometimes you may forget the protocol implementation or you may provide something that doesn&#8217;t match the method signature. If you see no meaningful errors coming from the compiler, upgrade ClojureScript.
    
    <pre class="brush: clojure; title: ; notranslate" title="">(defn some-component [cursor owner]
  (reify
    om/IRenderState
    (render [_])))
</pre>
    
    ClojureScript compiler warns you nicely about the mistake:
    
    <pre class="brush: clojure; title: ; notranslate" title="">WARNING: Bad method signature in protocol implementation, om/IRenderState does not declare method called render at line 159 src/cljs/pumpkin/core.cljs
</pre>
    
    I remember seeing java.lang.IndexOutOfBoundsException which was caused by missing protocol implementation. Upgrading helped. Try to stay up to date with both Om and ClojureScript. You&#8217;ll see more meaningful errors and warnings.</li> 
    
      * <p class="p1">
          <strong><span class="s1">Don&#8217;t do computations in render<br /> </span></strong>
        </p>
        
        If you need to group-by, or do other sorts of data transformations, it&#8217;s a bad idea to do it in render as it will slow it down. Try to transform your data as early as possible if the transformation is not caused by immediate user interaction. I usually do it in will-mount.</li> 
        
          * <p class="p1">
              <strong><span class="s1">Don&#8217;t create mult channel in render</span></strong>
            </p>
            
            Let&#8217;s say some parent component creates a number of components that depend on size of your data and you want all children to receive messages broadcasted on a core.async channel. Create a mult of that channel in init-state. Then, when children components are built in render, create a copy of that mult channel using tap. If you create mult in render all sorts of weirdness will happen.</li> 
            
              * <p class="p1">
                  <strong>Oh god, don&#8217;t use lazy sequences in cursors.</strong>
                </p>
                
                This one should be obvious, but if you map, remove or filter something in your data and update the cursor with the result, you will end up with a lazy sequence. Remember about mapv, filterv or into [].</li> </ol> 
                
                That&#8217;s all I remember. Hope it&#8217;s useful!