---
layout: post
title: Clojure, A Reflection On My Latest Obsession
categories:
  - code
  - clojure
  - quil
  - draw
  - art
  - animation
---

Every Friday we at Smashing Boxes take 4 hours out of the day to play explore our own personal interests and, for the past few months, I’ve been interested in Clojure. For the uninitiated, Clojure is a JVM based dialect of LISP. Clojure has a strong presence in the web development community via [ClojurScript](https://github.com/clojure/clojurescript). But, I’m not here to talk about ClojurScript. I’m here to talk about Clojure and how it’s re-introduced me to crazy but beautify world of LISP and functional programming.

For the past few years, I've been very "anti functional" for no good reason. Maybe it has been my almost-fanatic obsession with Ruby and Object-Oriented programming, but functional programming is starting to influence my own personal coding style. Which is strange because one of my first introductions into programming was with Scheme, which has long stood as the de-facto implementation of LISP and the go-to functional programming language.

Actually, my first intro to computer programing was from the TI-83 calculator handbook. It's brief chapter about TI BASIC which was the small programming language built into the TI-83.

![](/public/images/ti-83.jpg)

I remember the first program I ever wrote. It consisted of about 50 print statements and a loop. When you ran the code, *'s would bounce from one side of the screen to the other. I guess the first thing I wanted to to with code was make cool shit happen on a screen.

After I learned TI-Basic, some time passed and I got into graphics programming. I played around a bit with DirectX but I did not find it very enjoyable. So I abandoned that passion and settled on building websites that did cool shit in a window on my screen. But, my desire to write code that just "does random things that looks cool" never died. I guess I was just born with a passion to make screen savers.

## Quil

![](https://camo.githubusercontent.com/90bc972502b59f7b670dd3c249a7cfc9796f8d23/687474703a2f2f636c6f75642e6769746875622e636f6d2f646f776e6c6f6164732f7175696c2f7175696c2f7175696c2e706e67)

When a co-worker introduced me to [Quil](http://quil.info), I was intrigued. Quil is a carefully-crafted Clojure library built to make drawing and animation extremely easy. [There are lots of examples of awesome animations](http://quil.info/examples) written with the Quil library in Clojure that were well under 100 lines of code.

My pride a joy is a glitch-art effect I created that loads in an image, breaks it up into N 10x10 pixel squares, and then starts swapping those squares. This all started with me playing around with Quill’s [blend](http://quil.info/api/image/pixels#blend) function; trying out different effects and argument just to see what would come out on the other side. [You can see the results of those experiments here](https://imgur.com/a/DaZKh). After a few hours of experimenting, I landed on this effect and it turned out really cool!

![](https://cloud.githubusercontent.com/assets/362269/12542273/d19fbb94-c2ee-11e5-98a3-38a390632841.gif)

-and, the clojure code I wrote was crazy simple.

~~~clojure
(ns storefront.color-swap
  (:require [storefront.drawing]
            [storefront.helpers :refer :all]
            [quil.core :as q])
  (:import  [storefront.drawing Drawing]))

(def pixel-size 30)
(def pixel-count 5)

(defn blend-pixels [x y dx dy effect]
  (q/blend x y pixel-size pixel-size dx dy pixel-size pixel-size effect))

(defn pixel-array []
  (shuffle (for [x (range 0 (q/width) pixel-size)
        y (range 0 (q/height) pixel-size)]
    [x y])))

(defn end-of-pixel-array? [state]
  (= 0 (count (:pixel-array state))))

(defn load-new-image []
  (q/image (q/load-image (random-image-file)) 0 0 (q/width) (q/height)))

(defn setup [options]
  (load-new-image)
  {:pixel-array (pixel-array)
   :pixels-to-blend []
   :dpixels-to-blend []
   :effects [:blend]})

(defn draw-state [state]
  (if (:load-new-image state)
    (load-new-image))
  (let [pixels (:pixels-to-sample state)]
    (let [dpixels (:pixels-to-modify state)]
      (doseq [pixel (map vector pixels dpixels)]
        (let [x (get-in pixel [0 0])
              y (get-in pixel [0 1])
              dx (get-in pixel [1 0])
              dy (get-in pixel [1 1])]
          (blend-pixels x y dx dy (first (:effects state))))))))

(defn update-state [state]
  {:pixels-to-modify (take pixel-count (:pixel-array state))
   :pixels-to-sample (take pixel-count (drop pixel-count (:pixel-array state)))
   :pixel-array (if (end-of-pixel-array? state)
                  (pixel-array)
                  (drop pixel-count (:pixel-array state)))
   :load-new-image (end-of-pixel-array? state)
   :effects (if (end-of-pixel-array? state)
              (:effects state)
              (concat (rest (:effects state)) [(first (:effects state))]))})

(def drawing
  (Drawing. "Color Swap" setup update-state draw-state nil nil))
~~~

I plan on going into more detail about how to work with Quil but the premise is simple.

* A `setup` function defines the initial state of your animation.
* An `update` function modifies the state of your animation.
* A `draw` function draws the new step or frame of your animation based on the state you set in `update`.

## Clojure

Now, just take a moment and realize that all this was done by someone who has written next to no Clojure before. Yes, Clojure is quite difficult to read, which makes it scary to approach. But let me take a moment to lay some initial groundwork for you that may convince you to give Clojure a shot, if only to have some fun and play with Quil. All I ask you to do is force yourself to get past all those parenthesis. They really are not that big of a deal once you get used to Lisp’s crazy [Polish Style Notation](https://en.wikipedia.org/wiki/Polish_notation) (+ 3 4 vs 3 + 4).

### Leiningen
[Leiningen](http://leiningen.org/) is the first thing that you should download


~~~bash
brew install leiningen
~~~

Leiningen (lein) comes with some very basic commands that you can run to stand up the skeleton of a basic Clojure application and run it. Leiningen is, hands-down, the easiest way to get started with Clojure (outside of downloading some IDE like [Light Table](http://lighttable.com/) or emacs).
Once you have leiningen up, create a new project.

Once you have leiningen up, create a new project.

~~~bash
lein new app clojure-app
~~~

That command will create a basic directory-structure and some files that you can start to fill with Clojure code. You can also include 3rd party Clojure libraries and Leiningen will detect and install them for you.

When you have something written, you just run your project using leiningen.

~~~bash
lein run
~~~

Checkout their [Tutorial](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md) to find out more.

### Clojure Docs
Clojure's docs are likely some of the best that I have ever seen. The reason for that is because they are chock full of code examples that have been submitted, modified, and curated by the community.

Just take a look at the documentation for the [reduce function](https://clojuredocs.org/clojure.core/reduce). If only all languages had a doc website like this!

### Try Clojure
Obviously, you may want to try out a little Clojure yourself before you start to download leiningen and create your first project. Welp, you **could** just run `lein repl` and immediately drop into a Clojure cli, but I think you should start by visiting one of the following websites:


* [Try Clojure](http://www.tryclj.com/)
* [exercism](http://exercism.io/languages/clojure)

## Pros and Cons

While Clojure is a ton of fun, it is not without its drawbacks. Here are a few things that I liked and a few that I did not.

### Pros
* Excellent Docs
* Crazy fast
* Familiar syntax for LISP people
* Personable, able, passionate community
* The repl (hot-update your code while it is running to see changes immediately)

### Cons
* Sparse choice of Rails-like frameworks. Clojure people love micro-frameworks and they like to have a lot of control over the structure of their projects.
* Oddly-named functions. If you Google “Clojure Map” you will get results for the map function, not the `{:a 1 :b 2}` kind of map that you may be thinking about.
* JVM. The JVM is a beast to start-up. But, once you understand how to use the REPL, that is no problem. However, even after a few weeks in Clojure, I found the REPL difficult to use.

## My Conclusion

I probably won't be writing a web API in Clojure anytime soon, but I love having a tool that does cool stuff outside the web world that is so easy to work within. Not everything needs to be for work, sometimes we just code for fun. And Clojure certainly is fun!
