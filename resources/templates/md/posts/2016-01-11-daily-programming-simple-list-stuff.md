{:title "daily programming simple list stuff"
:layout :post
 :tags ["old-post"]}



Since I'm warming up to Clojure this year, why not get started with the "dailyprogrammer" subreddit again?



So here's a (noobish) solution to [this problem](https://www.reddit.com/r/dailyprogrammer/comments/40h9pd/20160111_challenge_249_easy_playing_the_stock/):



```clojure

(ns dailyprog.core

  (:gen-class)

  (:require [clojure.string :as str]))



(defn input->float

  [input]

  (map #(Float/parseFloat %) (str/split input #" ")))



(defn difference

  "Given a list of prices, a price in the list and its index, return the best price to sell it at"

  [lst index buy-price]

  (let [max-sell-price (apply max (drop index lst))]

    {:buy-price buy-price

    :sell-price max-sell-price

    :difference (- max-sell-price buy-price)}))



(defn difference-vector

  [lst]

  (map-indexed (partial difference lst) lst))



(defn -main

  [& args]

  (println(->> (slurp (first args))

                input->float

                difference-vector

                (apply max-key :difference))))



```



and it runs like this:



```sh

> lein run /tmp/challenge-input

{:buy-price 8.03, :sell-price 10.02, :difference 1.9900007247924805}

```


