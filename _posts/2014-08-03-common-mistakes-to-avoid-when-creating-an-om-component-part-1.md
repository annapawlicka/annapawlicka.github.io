---
title: Common mistakes to avoid when creating an Om component. Part 1.
author: Anna
layout: post
permalink: /common-mistakes-to-avoid-when-creating-an-om-component-part-1/
views:
  - 4612
categories:
  - Clojure
---
For the past few months I've been creating various Om components and most of the time it goes smoothly. But sometimes I do something silly, and it's not always obvious what it is. What's obvious is that the UI doesn't work and throws (sometimes cryptic) errors at me instead. Today I'm gathering what I remember into a tiny post. Maybe someone will find it helpful. And maybe I'll finally remember those mistakes after putting them down on <del>paper</del> interwebs. One can hope!  <img src="http://annapawlicka.com/wp-includes/images/smilies/icon_smile.gif" alt=":-)" class="wp-smiley" />

1. <strong>Form inside of another form</strong>

    ```JavaScript
    Error: Invariant Violation: findComponentRoot(..., .2): Unable to find element. This probably means the DOM was unexpectedly mutated (e.g., by the browser), usually due to forgetting a <tbody> when using tables or nesting <p> or <a> tags. Try inspecting the child nodes of the element with React ID
    ```
    
    Easy crime to commit if you're moving some html tags around and leave one behind. [GitHub issue](https://github.com/facebook/react/issues/1665).
      
2. <strong>Reading application state during user input, e.g. onClick, onChange. etc.</strong>

    ```JavaScript
    Uncaught Error: Cannot manipulate cursor outside of render phase, only om.core/transact!, om.core/update!, and cljs.core/deref operations allowed.
    ```
   
    You need to deref to read the value: `(:some-value @cursor)`
          
3. <strong>Same error as above but coming from your `om/transact!`</strong>

    Common mistake in the function that you pass to `om/transact!` is to do something like this:
    
    ```clojure
    (om/transact! data :messages (fn [cursor] (conj (:messages data) some-value))
    ```
    
    Cursor is actually your `(:messages data)` so you just do:
    
    ```clojure
    (fn [cursor] (conj cursor some-value))
    ```
4. <strong>React.js < v. 0.11.0 Render method not returning a valid component</strong>

    ```JavaScript
    Uncaught Error: Invariant Violation: ReactCompositeComponent.render(): A valid ReactComponent must be returned. 
    You may have returned null, undefined, an array, or some other invalid object.
    ```
    You *must* return a single div. It might contain nested stuff, other divs, components, etc. but everything must be wrapped in a single div.
    And if you want to return nothing because you have no data to display, you still need to return an empty div from your render method.
    Starting from React 0.11.0 you are free to return null. Thanks <a href="https://twitter.com/locks">@locks</a> for pointing this out!</li> </ol>
              

 And an obvious tip: don't use a minified build of React library for development if you want to see meaningful error descriptions.
 
 I'll add more pitfalls as I remember them.
