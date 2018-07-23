{:title "euler 23 non abundant sums"
:layout :post
 :tags ["old-post"]}



Had a slice of free time, so decided to look at the next problem. Here's the initial naive solution:



```haskell

divisors n = [x | x <- [1 .. n-1], n `mod` x == 0]



isAbundant n = n < (sum $ divisors n)



abundants n = filter isAbundant $ [1 .. n]



expressSum z nums = not $ null [(x,y) | x <- nums, y <- nums, x + y == z]



sumAbundants n =

  let a = abundants n in

          sum $ filter (\x -> not $ expressSum x a) [1 .. n]

```



While it gave the correct answer, it did so in `3133.33` seconds, which is ... embarrassing.



This is just _too_ inefficient, even the following ...



```lisp

(defun divisors (n)

  (loop for i from 1 below n

     when (= 0 (mod n i))

       collect i))



(defun abundantp (n)

  (< n

     (reduce #'+ (divisors n))))



(defun abundants (n)

  (loop for i from 1 below n

     when (abundantp i)

       collect i))



(defun possible-summands (z nums)

  (loop for x in nums do

       (loop for y in nums

	  when (= z (+ x y)) do

	    (return-from possible-summands (cons x y)))))

  

(defun sum-abundants (n)

  (let* ((ab (abundants n))

	 (summands 

	  (loop for i from 1 to n

	     when (null (possible-summands i ab))

	     collect i)))

    (reduce #'+ summands)))

```



... takes no less than half as long, at `1329.168` seconds.



BTW why is `28123` the upper limit for this sequence? I had no idea, and found [this explanation](http://mathschallenge.net/full/sum_of_two_abundant_numbers) (and tangentially, [this one](http://mathschallenge.net/view/even_sum_of_two_abundant_numbers) too).



Anyway, I'm ashamed to say I didn't put in the effort to learn how to profile Haskell programs (_next time ?_) and profiled the Lisp version instead, which showed that (**duh**) all the time was going in finding divisors. Obviously, we can just loop till the _square root of N_ instead of looping _all the way to N_. After this change:



```lisp

(defun divisors (n)

(declare (type fixnum n))

  (loop for i from 1 below (floor (sqrt n))

     when (= 0 (mod n i))

       collect i))

```



... it runs in `38 milliseconds` !!



And a similar change to the Haskell version:



```haskell

divisors :: Int -> [Int]

divisors n = [x | x <- [1 .. round $ sqrt $ fromIntegral n], n `mod` x == 0]

```



gave the expected answer in `180 milliseconds`. Not bad (though it should be noted we're still an order of magnitude away in efficiency).



**Notes:**



- Haskell makes it very easy to quickly arrive at a _correct_ solution, but the road from there to an _efficient_ solution is less clear.



- The brevity of the notation helps, but I get a feeling it's also due to the single-letter variable names -- which is all right (and indeed well-suited) for a tiny math problem, but unclear whether it'll hold up for code in a large-scale project.



- People usually jump to the ["Programming Language shootout"](), but if the code _there_ is any indication, writing performant Haskell code is a dark art, and _the three-line quicksort is a devilish honeytrap_.
