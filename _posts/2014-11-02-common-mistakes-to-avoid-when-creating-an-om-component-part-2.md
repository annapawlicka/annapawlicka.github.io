---
title: Common mistakes to avoid when creating an Om component. Part 2.
author: Anna
layout: post
permalink: /common-mistakes-to-avoid-when-creating-an-om-component-part-2/
views:
  - 1427
tags:
  - clojure
  - ClojureScript
---
It's been a while since the last post. More mistakes have been made, lessons have been learned, so here's a handful:

  1. <strong>Combining cursors</strong>
    
    Make sure you're updating the cursor and not the map that combines them.
    Let's say you want to create a component that has a view of bands and titles of films. You don't want to pass whole of the app state,
    you just want to pass `:albums` and `:titles`.
    You combine them like this:
    
    ```clojure
    (def app-state {:music {:artists []
                            :bands {:faved []}
                            :albums {:faved []}}
                   :films {:directors []
                           :titles {:faved []}}})
    ```

    ```clojure
    (om/build select-favourites {:bands  (-&gt; cursor :music :bands)
                                 :titles (-&gt; cursor :films :titles)})
    ```
    
    Inside of you `select-favourites` component you might assume that cursor is an actual cursor. It's not, it's just a map containing cursors. You can't do this:
    
    ```clojure
    (defn select-favourites [cursor owner]
    (om/component
    ...
    (om/transact! cursor [:bands :faved] #(conj % some-band))))
    ```
    
    You can't update the map. You need to get the cursor out of the map:
    
    ```clojure
    (defn select-favourites [{:keys [bands titles]} owner]
    (om/component
    ...
    (om/transact! bands :faved #(conj % some-band))))
    ```


  2. <strong>Method signature and protocol implementation</strong>

    
    Sometimes you may forget the protocol implementation or you may provide something that doesn't match the method signature.
    If you see no meaningful errors coming from the compiler, upgrade ClojureScript.
    
    ```clojure
    (defn some-component [cursor owner]
    (reify
      om/IRenderState
        (render [_])))
    ```
    
    ClojureScript compiler warns you nicely about the mistake:
    
    ```JavaScript
    WARNING: Bad method signature in protocol implementation, om/IRenderState does not declare method called render at line 159 src/cljs/pumpkin/core.cljs
    ```
    
    I remember seeing `java.lang.IndexOutOfBoundsException` which was caused by missing protocol implementation. Upgrading helped.
    Try to stay up to date with both Om and ClojureScript. You'll see more meaningful errors and warnings.
    
  3. <strong>Don't do computations in render</strong>
        
    If you need to group-by, or do other sorts of data transformations, it's a bad idea to do it in render as it will slow it down.
    Try to transform your data as early as possible if the transformation is not caused by immediate user interaction.
    I usually do it in `will-mount`.
        
  4. <strong> Don't create mult channel in render</strong>
            
    Let's say some parent component creates a number of components that depend on size of your data and you want all
    children to receive messages broadcasted on a core.async channel. Create a mult of that channel in init-state. T
    hen, when children components are built in render, create a copy of that mult channel using tap. If you create mult in render 
    all sorts of weirdness will happen.

  5. <strong> Oh god, don't use lazy sequences in cursors.</strong>

    This one should be obvious, but if you map, remove or filter something in your data and update the cursor with the result,
    you will end up with a lazy sequence. Remember about `mapv`, `filterv` or `into []`.

That's all I remember. Hope it's useful!