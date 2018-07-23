{:title "euler 30 sums of powers of digits"
:layout :post
 :tags ["old-post"]}



Another quick and easy solution. This doesn't mean it's an _easy problem!_; no, it's more like playing a level at the _rookie level_. The hard and "correct" solution would be to work it out on pen-and-paper using Number Theory.



**Statutory Warning**: _Spoilers ahead_



```haskell

digits :: Int -> [Int]

digits n = digitsHelper n []

  where

    digitsHelper num dList =

      if num > 0

      then digitsHelper (div num 10) $ (mod num 10) : dList

      else dList



sumFifthPow :: Int -> Int

sumFifthPow n = sum $ map (^ 5) $ digits n



euler30 :: Int -> Int

euler30 limit = sum $ [x | x <- [2 .. limit], x == sumFifthPow x]

```



Again, this version needs "manual intervention", since I used the rough approximation that if `euler30 1000000` and `euler30 10000000` gave the same result, this was probably the right answer. It was.
