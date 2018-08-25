{:title "Using Clojure for simple math"
 :layout :post
 :tags ["clojure"]}

Someone I respect recommended _Brilliant_ to me, and it turned out to be fun indeed; wish I had encountered it sooner.

Lots of little examples, to play with, and to learn from. I was stuck at something else today, so wrote a tiny program for one of these.

The [problem](https://brilliant.org/practice/is-it-possible/?p=2) is extremely simple (this post is more about using a flexible programming language _recreationally_, nothing else):

Are there basic arithmetic operators (one of `+`, `-`, `*`, `/`) that can be inserted into the blanks to get the desired result?

> 10 \_\_ 10 \_\_ 10 \_\_ 10 = 100

So, a simple, brute-force solution:

```clojure
(ns clj-playground.core
  (:require [clojure.math.combinatorics :as combo]))

(def n 10)
(def ops [+ - * /])

;; Evaluate a given set of operators.
(defn compute
  [n opv]
  (reduce (fn [v op] (op v n))
		  n
		  opv))

;; Generate all possible 3-tuples of operators
(defn all-variations
  []
  (mapcat identity
   (map clojure.math.combinatorics/permutations 
		(clojure.math.combinatorics/combinations ops 3))))

(defn find-values [val]
  []
  (->> (all-variations)
	   (map #(vec [% (compute n %)]))
	   (filter (fn [[o v]] (= v val)))
	   (clojure.pprint/pprint)))

```

And to use it:

```clojure
user> (clj-playground.core/find-values 100)
([(#function[clojure.core/+]
   #function[clojure.core/-]
   #function[clojure.core/*])
  100]
 [(#function[clojure.core/-]
   #function[clojure.core/+]
   #function[clojure.core/*])
  100]
 [(#function[clojure.core/*]
   #function[clojure.core/+]
   #function[clojure.core/-])
  100]
 [(#function[clojure.core/*]
   #function[clojure.core/-]
   #function[clojure.core/+])
  100])
```

So, there _are_ ways to get there, and here they are.

I know, I know, toy example, but still.

