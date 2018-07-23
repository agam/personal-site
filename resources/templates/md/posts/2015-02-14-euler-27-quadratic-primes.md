{:title "euler 27 quadratic primes"
:layout :post
 :tags ["old-post"]}



**Statutory Warning**: Spoilers ahead



_This turned out to be really simple, though I went through a phase of churning out complicated solutions because of a stupid mistake._



I didn't preserve intermediate versions, so here is the final solution:



```haskell

import qualified Data.List as L

import qualified Data.Vector as V

import qualified Data.Function as F



sieve :: Int -> [Int] -> [Int]

sieve num list =

  if null list

  then [num]

  else let rest = filter (\x -> x `mod` num > 0) list

       in

        num : sieve (head rest) (tail rest)



eratosthenes :: Int -> [Int]

eratosthenes maxNum = sieve 2 [3 .. maxNum]



allPrimes :: Int -> V.Vector Int

allPrimes maxNum = V.fromList $ eratosthenes maxNum



eulerEqn :: Int -> Int -> Int -> Int

eulerEqn a b n = n * n + a * n + b



-- TODO(agam): replace with the "standard" way to do binary search

binSearch :: V.Vector Int -> Int -> Int -> Int -> Bool

binSearch arr min max elem =

  let low = arr V.! min

      high = arr V.! max

  in

   if max - min < 2

   then if low == elem

        then True

        else False

   else let mid = div (min + max) 2

            midElem = arr V.! mid

        in

         if midElem > elem

         then binSearch arr min mid elem

         else binSearch arr mid max elem



consecutivePrimes a b primes start length =

  let p = eulerEqn a b start

      l = V.length primes

      isPrime = binSearch primes 0 l p

  in

   if isPrime

   then consecutivePrimes a b primes (start + 1) (length + 1)

   else length



numPrimes a b primes = consecutivePrimes a b primes 0 0



euler27 maxNum =

  let ap = allPrimes maxNum

      primeLengths = [(a * b, numPrimes a b ap) | a <- [-1000 .. 1000], b <- [-1000 .. 1000]]

  in

   L.maximumBy (F.on compare snd) primeLengths

```



When I ran this (i.e. `euler27 1000`) I got the correct answer the first time! But _I didn't see it_. Instead, I entered the _second_ value of the tuple, which is _not_ the one asked for, and it was therefore _obviously_ wrong. So I thought "hmm, we're looking at some large repeating sequence among really large primes", and tried `euler27 100000` and `euler27 10000000`, with no luck.



The last one kept running for hours and I killed it, and then came up with the idea of "vectorizing everything" -- which was perhaps a good academic exercise but did absolutely _nothing_ for the performance here.



So I forgot about it for a while, then came back and ran `euler27 1000`, entered the _first_ value of the tuple, and that was the end of this story.
