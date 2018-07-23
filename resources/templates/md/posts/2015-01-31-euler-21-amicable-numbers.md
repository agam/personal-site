{:title "euler 21 amicable numbers"
:layout :post
 :tags ["old-post"]}



_Picking up where I left off, apparently 2 years ago !! :(_



To avoid a procrastination excuse, I decided to skip the step of adding it to the github repo I had started earlier. Here is the raw, unedited form of the attempt at the next problem in my sequence, #21.



```haskell

isDiv a b = a `mod` b == 0



divisors n = [x | x <- [1 .. n-1], isDiv n x]



-- d(n) = sum of divisors of n

sumDiv n = sum $ divisors n



-- a and b are 'amicable' if d(a) = b, and d(b) = a

amicable a b = (a /= b) && (sumDiv a == b) && (sumDiv b == a)



-- evaluate all pairs under 1000

-- amicableUnder1000 = [(x,y) | x <- [1 .. 999], y <- [1 .. 999], amicable x y]

-- euler21 = foldr (\(x,y) -> x + y) 0  amicableUnder1000

```



This naive version was predictably going to take forever, so interrupted Ghci and decided to try a slightly different approach (to explicitly pre-evaluate the sums of divisors).



```haskell

sumDivNumbersUnder10000 = [sumDiv n | n <- [1 .. 9999]]

sumDiv' n = sumDivNumbersUnder10000 !! (n-1)



amicable' (a,sa) (b,sb) = (a /= b) && (sa == b) && (sb == a)



amicableUnder10000 = [(x,y) | x <- [1 .. 9999], y <- [1 .. 9999], amicable' (x,sumDiv' x) (y, sumDiv' y)]

```



This version took `4.64` seconds in Ghci for the numbers less than 1000, at which point I realized the problem had actually called for the numbers less than _10000_ instead. I left it running, out of curiosity, and it took `1758.96` seconds (not to mention a _prodigious_ amount of memory: `97202052160` bytes!)



This done, the final answer was easy:



```haskell

sum $ map fst amicableUnder10000

31626

```



But this sort of gets to my problem with Haskell; I'm never sure what's _really_ going on, and how to make it do _what I want it to do_. On the other hand, I can feel an excess of "imperative thinking" is getting in the way (why isn't this as fast as a nested for loop?) Obviously, I need more time at this :)



Anyway, I tried yet another way towards this:



```haskell

partialAmicable x n = [(x,y) | y <- [1 .. n-1], amicable' (x, sumDiv' x) (y, sumDiv' y)]



fullAmicable n = filter (not . null) (map f $ take n [1 ..])

             where f x = partialAmicable x n

```



... which ran into `*** Exception: Prelude.(!!): index too large`



At this point I realized that:



- I had no idea how to 'debug' this (lacking a 'stack trace'), but also

- I was still dealing with **lists**, when I really wanted **vectors**.



So I gave it one _final_ shot:



```haskell

import Data.Vector as V



divSums n = V.fromList [sumDiv x | x <- [1 .. n-1]]



amicables n = let ds = divSums n in

                  V.fromList [(x,y) | x <- [1 .. n-1], y <- [1 .. n-1], amicable' (x, ds ! (x-1)) (y, ds ! (y-1))]

```



... and this time, I got



```haskell

λ> amicables 10000

fromList [(220,284),(284,220),(1184,1210),(1210,1184),(2620,2924),(2924,2620),(5020,5564),(5564,5020),(6232,6368),(6368,6232)]

(204.01 secs, 97203591888 bytes)

```



Ok, I can stop here; perhaps `204` seconds of brute-forcing isn't all that bad?



Unfortunately, my "comfort zone" yielded this:



```lisp

(defun div (x y)

  (= (mod x y) 0))



(defun divisors (x)

  (loop for i from 1 below x

     when (div x i)

       collect i))



(defun sum-divs (x)

  (reduce #'+ (divisors x)))





(defun pre-sum-divs (n)

  (let ((myarr (make-array n :element-type 'fixnum)))

    (loop for i from 1 below n

	 do (setf (aref myarr i) (sum-divs i)))

    myarr))





(defun amicablep (x sx y sy)

  (and (not (= x y))

       (= sx y)

       (= sy x)))



(defun amicables (n)

  (let ((ds (pre-sum-divs n)))

    (loop for i from 1 below n do

	 (loop for j from 1 below n

	    when (amicablep i (aref ds i) j (aref ds j))

	    do (print j)))))



```



which runs _just a little bit faster_ (!!)



```lisp

CL-USER> (time (amicables 10000))



284

220

1210

1184

2924

2620

5564

5020

6368

6232

Evaluation took:

2.815 seconds of real time

2.820000 seconds of total run time (2.820000 user, 0.000000 system)

100.18% CPU

7,318,463,192 processor cycles

3,939,984 bytes consed

```



So this is my problem: I need to find a way to get my mental model of Haskell to perform at this speed (and clearly, it's two orders of magnitude off). It isn't going to be easy ...



**EDIT**: Make that just _one_ order of magnitude.

It's possible for Ghci to use compiled object code instead of byte code, by entering `:set -fobject-code`.

After this, evaluating `amicables 10000` took `23.71` seconds.
