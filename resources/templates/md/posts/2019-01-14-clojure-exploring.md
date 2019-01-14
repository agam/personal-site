
{:title "Clojure Exploration" :layout :post :tags [""]}

Some stuff I found useful when tyring to look up stuff in new namespaces.

```clojure
(defn info
  "Show `type` and `class` of an object"
  [x]
  (println x ": type (" (type x) "), class (" (class x) ")"))

(defn get-vars
  [n]
   (map #(ns-resolve n %1) (clojure.repl/dir-fn n)))

(defn show-doc
  [v]
  (vector (:name (meta v))
          (:doc (meta v))))

(defn get-docs
  [n]
  (->> (get-vars n)
       (map show-doc)
       (pprint)))
```
