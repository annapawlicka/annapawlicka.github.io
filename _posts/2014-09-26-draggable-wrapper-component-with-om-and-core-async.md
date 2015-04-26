---
title: Draggable wrapper component with Om and core.async
author: Anna
layout: post
permalink: /draggable-wrapper-component-with-om-and-core-async/
switch_like_status:
  - 1
views:
  - 1829
categories:
  - Clojure
  - ClojureScript
tags:
  - ClojureScript
  - Om
  - React
---
I&#8217;ve been looking for a way to enable dragging of Om components, something similar to what [Draggable][1] does but much much simpler. I just want to drag component around the UI, no bells and whistles. I didn&#8217;t want to add this functionality to each component but just enable it as needed. Hence a wrapping component.

It&#8217;s very simple: the wrapper has a core.async channel that event listeners are writing to:

<pre class="brush: clojure; title: ; notranslate" title="">(defn draggable [cursor owner {:keys [build-fn id]}]
  (reify
    om/IInitState
    (init-state [_]
      {:mouse-chan (chan (sliding-buffer 100))
       :pressed false})
    om/IWillMount
    (will-mount [_]
      (let [mouse-chan (om/get-state owner :mouse-chan)]
        (go-loop []
          (let [[evt-type e] (&lt;! mouse-chan)]
            (handle-drag-event cursor owner evt-type e))
          (recur))))
    om/IDidMount
    (did-mount [_]
      (let [node       (by-id id)
            mouse-chan (om/get-state owner :mouse-chan)]
        (events/listen node "mousemove" #(put! mouse-chan [:move %]))
        (events/listen node "mousedown" #(put! mouse-chan [:down %]))
        (events/listen node "mouseup" #(put! mouse-chan [:up %]))))
    om/IRenderState
    (render-state [_ {:keys [mouse-chan]}]
      (html
       (let [{:keys [top left]} (:position cursor)]
         [:div {:id id
                :style {:top (str (- top 40) "px") :left (str (- left 40) "px")
                        :position "absolute" :z-index 100}}
          build-fn])))))
</pre>

Channel and default mouse pressed value are initialised in *IInitState*. Channel has a sliding buffer &#8211; this way when someone drags too fast we don&#8217;t update app state unnecessarily but drop the events instead.  
In *IDidMount* we attach listeners to our component and &#8220;mousemove&#8221;, &#8220;mousedown&#8221; and &#8220;mouseup&#8221; events. The handler is simply putting a vector with the event type and the event object on the mouse-chan channel. Inside of *IWillMount* we have a go-loop that reads the messages and handles the events according to their type:

<pre class="brush: clojure; title: ; notranslate" title="">(defn handle-drag-event [cursor owner evt-type e]
  (when (= evt-type :down)
    (om/set-state! owner :pressed true))
  (when (= evt-type :up)
    (om/set-state! owner :pressed false))
  (when (and (= evt-type :move) (om/get-state owner :pressed))
    (om/update! cursor :position {:top (.-clientY e) :left (.-clientX e)})))
</pre>

On mouse down and up, we update component&#8217;s local state accordingly &#8211; we don&#8217;t want to act on mouse move if the mouse is not pressed. On mouse move we simply update the cursor with x and y coordinates of the mouse, which causes the component to render at new position.

How does the draggable know what component to render? We pass the whole *(om/build box (:draggable-box cursor))* as one of the options to draggable:

<pre class="brush: clojure; title: ; notranslate" title="">(defn draggable-widget [cursor owner]
  (reify
    om/IRenderState
    (render-state [_ state]
      (html
       [:div {:class "container"}
        (om/build w/draggable (:draggable-box cursor) {:opts {:id "box-widget"
                                                              :build-fn (om/build box (:draggable-box cursor))}})]))))

</pre>

<a href="http://annapawlicka.github.io/om-data-vis/public/draggable-widget/index.html" target="_blank">Here&#8217;s</a> the working example. You&#8217;ve probably noticed that when you drag the component too fast, the cursor moves, but the component remains in the same place (buffer drops the events). It would be better to just take the last position of the cursor and move the component there. But I&#8217;ll let you figure this one out yourself <img src="http://annapawlicka.com/wp-includes/images/smilies/icon_wink.gif" alt=";-)" class="wp-smiley" />

You can find the code in my <a href="https://github.com/annapawlicka/om-data-vis" target="_blank">GitHub repo</a>. Let me know if you find bugs, or better yet, submit at PR!

 [1]: http://jqueryui.com/draggable/