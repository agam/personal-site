{:title "clojure from the ground up"
:layout :post
 :tags ["old-post"]}



It's time for the same old new year resolution again: going to stop randomly skimming new languages and focus on something in depth.



This time, without loss of generality (hopefully!), I picked Clojure (_again ?_).



Found this nice series of posts called [_"Clojure from the ground up"_](https://aphyr.com/posts/301-clojure-from-the-ground-up-welcome).



The best part is that unlike other random free tutorials online, this one has exercises.



For example, [the post that introduces concurrency primitives](https://aphyr.com/posts/306-clojure-from-the-ground-up-state) asks us to write our own `future` macro.



I came up with this kludge:



```clojure

(defmacro my-future [& body]

  `(let [p# (promise)]

    (.start (Thread.

      (fn [] (let [f# (fn [] ~@body)

                   r# (f#)]

               (deliver p# r#)))))

               p#))

```



The expansion looks like this:



```clojure

clojure-noob.core> (pprint (macroexpand '(my-future (Thread/sleep 5000) (prn "Hey ... "))))

(let*

  [p__9904__auto__ (clojure.core/promise)]

  (.start

    (java.lang.Thread.

      (clojure.core/fn

       []

       (clojure.core/let

        [f__9905__auto__

          (clojure.core/fn [] (Thread/sleep 5000) (prn "Hey ... "))

          r__9906__auto__ (f__9905__auto__)]

        (clojure.core/deliver p__9904__auto__ r__9906__auto__)))))

        p__9904__auto__)

nil

```



And it seems to work:



```clojure

clojure-noob.core> (def x (my-future (Thread/sleep 5000) (prn "Hey ... ")))

#'clojure-noob.core/x

clojure-noob.core> (pprint x)

#<Promise@6a1dc62: :not-delivered>

nil

"Hey ... "

clojure-noob.core>

clojure-noob.core> (pprint x)

#<Promise@6a1dc62: nil>

nil

```



Here's hoping to fun times ahead (and a _happy new year_ to you too!)
