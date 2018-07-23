{:title "some badly written code"
:layout :post
 :tags ["old-post"]}



Heh, didn't know what else to title this sort of scrapbook/notebook entry. Basically I hadn’t looked at Codewars for a long time, so I went back and tried the next “Kata”.



_Problem:_ Given two integers m, n (1 <= m <= n) we want to find all integers between m and n whose sum of squared divisors is itself a square.



(E.g. 42 has divisors: 1,2,3,6,7,14,21,42, the squares of which are 1,4,9,3649,196,441,1764, and sum to 2500, which is a square)



I wrote my trivial solution, tried it and the submission failed because it timed out. So I hacked away, and uglified my solution, until it was using “memoized” divisors.



I ran it locally and it seemed faster, then I submitted it, and … it timed out again. I gave up, and moved on. I guess the lesson to be learnt is that it's always easy to code yourself into a corner? 



_Solution:_



```haskell

module Codewars.G964.Sumdivsq where



import Data.List

import Data.Map as M



intSqrt :: Int -> Int

intSqrt = floor . sqrt . fromIntegral



isSquare :: Int -> Bool

isSquare x = x == (intSqrt x)^2



sumSq :: [Int] -> Int

sumSq list = sum [x^2 | x <- list]



multiply :: Int -> [Int] -> [Int]

multiply factor oldDivlist = factor : oldDivlist ++ (Data.List.map (* factor) oldDivlist)



divisorHelper :: Int -> Int -> Int -> (Map Int [Int]) -> [Int] -> (Map Int [Int], [Int])

divisorHelper n lower upper knownDivs listDivs =

  if lower > upper

  then (M.insert n listDivs knownDivs, listDivs)

  else

    let otherDiv = n `div` lower

    in

      if (n `rem` lower /= 0)

      then

        -- Keep going till we can divide

        divisorHelper n (lower+1) upper knownDivs listDivs

      else

        if otherDiv == lower

        then

          -- Special case: we reach a square divisor

          (M.insert n (lower : listDivs) knownDivs, lower : listDivs)

          -- Ok, we need to know if we've seen the bigger number before

        else case M.lookup otherDiv knownDivs of

          Just oldDivlist ->

            -- We're done!

            let newDivlist = nub $ (lower : listDivs) ++ (multiply lower oldDivlist)

            in

              (M.insert n newDivlist knownDivs, newDivlist)

          Nothing -> divisorHelper n (lower+1) (otherDiv-1) knownDivs (lower : otherDiv : listDivs)





divisors :: Int -> (Map Int [Int]) -> (Map Int [Int], [Int])

divisors n knownDivs = divisorHelper n 1 n knownDivs []



listSquaredHelper :: Int -> Int -> Map Int [Int] -> [(Int, Int)] -> [(Int, Int)]

listSquaredHelper lower upper knownDivs sqList =

  if lower > upper

  then

    sqList

  else

    let (newKnownDivs, divs) = divisors lower knownDivs

        s = sumSq divs

    in

      if isSquare s

      then

        listSquaredHelper (lower+1) upper newKnownDivs  ((lower,s):sqList)

      else

        listSquaredHelper (lower+1) upper newKnownDivs sqList



listSquared :: Int -> Int -> [(Int, Int)]

listSquared m n = reverse $ listSquaredHelper m n M.empty []



```



I clearly have a long way to go in understanding the “why” of Haskell performance. My initial solution was much, uh ... simpler. I didn't save it but I translated that into Clojure, which looks something like this:



```clojure

(ns sumdivsq.core)



(defn is-square

  [n]

  (== n (Math/pow (int (Math/sqrt n)) 2)))



(defn sum-sq

  [lst]

  (int (reduce + (map #(Math/pow % 2) lst))))



(defn divisors

  [n]

  (if (== n 1)

    [1]

    (conj (filter #(== 0 (mod n %)) (range 1 (inc (/ n 2)))) n)))



(defn list-squared

  [m n]

  (letfn [(lfh [n]

            (let [ssq (sum-sq (divisors n))]

              (when (is-square ssq)

                [n, ssq])))]

    (keep #(lfh %) (range m n))))

```



Certainly _looks_ very nice, and it passed all the tests, but I was too impatient to begin optimizing it, and left this one behind too ...
