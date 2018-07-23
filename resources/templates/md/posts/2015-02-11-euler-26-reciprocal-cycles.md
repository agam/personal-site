{:title "euler 26 reciprocal cycles"
:layout :post
 :tags ["old-post"]}



**Statutory Warning**: Spoilers ahead



_Problem: find the number less than 1000 with the largest repeating cycle of digits in the decimal expansion of its reciprocal_



As always, the initial reacion was on the lines of _aha! this is obvious!_:



```lisp

(defun get-repeated-substring-length (str len)

  (let* ((rev-str (reverse str))

	 (offset (- (length str) (* 2 len)))

	 (match (equal (subseq rev-str 0 len)

		       (reverse (subseq str offset (+ offset len))))))

    (if match

	len

	0)))





(defun get-cycle-length (dec)

  (declare (type double-float dec))

  (let* ((dec-str (princ-to-string dec))

	 (exp-str (subseq dec-str

			  (1+ (position #\. dec-str))

			  (position #\d dec-str))))

    (do ((i 0 (1+ i))

	 (cycle-length 0 (get-repeated-substring-length exp-str i)))

	((or (> cycle-length 0) (> i (/ (length exp-str) 2))) cycle-length))))





(defun inverse-cycle-digits (n)

  (let ((inverse (/ 1.0 (coerce n 'double-float))))

    (do* ((i 0 (1+ i))

	 (dec (* 1.0 inverse) (* 10.0 dec))

	 (cycle-length 0 (get-cycle-length dec)))

	 ((> cycle-length 0) cycle-length))))



(defun euler-26-fail ()

  (let ((all-cycles (mapcar #'inverse-cycle-digits (loop for i from 1 to 1000 collect i))))

    (reduce #'max all-cycles)))

```



But the aptly named `euler-26-fail` fails to do what we want, because once again, the largest reciprocal when expanded fully, lies beyond the standard _double-float_ range. So, we hand-roll ...



```lisp



(defun inverse (n &key (precision))

  (do* ((i 0 (1+ i))

	(numerator 10 (* 10 (mod numerator n)))

	(dec (floor (/ 10 n)) (floor (/ numerator n)))

	(declist (list dec) (cons dec declist)))

       ((> i precision) (nreverse declist))))



(defun cycle-length-helper (declist)

  (do ((i 0 (1+ i))

       (cycle-length 0 (get-repeated-substring-length declist i)))

      ((or (> cycle-length 0) (> i (/ (length declist) 2))) cycle-length)))



(defun cycle-length (declist)

  (loop for length in (loop for i from 1 to (length declist)

			 collect (cycle-length-helper (subseq declist 0 i)))

       maximizing length))



(defun get-all-inverse-lengths (lst max-precision)

  (mapcar #'(lambda (n) (cycle-length (inverse n :precision max-precision)))

	  lst))



(defun unique-max (list)

  (let ((max (reduce #'max list)))

    (if (= 1 (count max list))

	max

	0)))



(defun biggest-cycle (max-num prec)

  (let* ((lst (loop for i from 1 to  max-num collect i))

	 (lengths (get-all-inverse-lengths lst prec))

	 (max-len (reduce #'max lengths)))

    ;;(format t "Debug: lengths = ~A, max = ~A~%" lengths max-len)

    (nth (position max-len lengths) lst)))

```



The idea is to loop over all numbers with a given precision, find the max, then maybe increase the precision and try again. This is a terrible idea, relying on the ability to submit multiple answers at the Project Euler website :(



But more than that, it's terribly slow. It becomes marginally faster if we restrict our search to primes.



```lisp

(defun biggest-cycle (max-num prec)

  (let* ((lst (eratosthenes max-num))

	 (lengths (get-all-inverse-lengths lst prec))

	 (max-len (reduce #'max lengths)))

    ;;(format t "Debug: lengths = ~A, max = ~A~%" lengths max-len)

    (nth (position max-len lengths) lst)))



(defun sieve (prime list)

  (if (null list)

      (list prime)

      (cons prime (sieve (first list)

			 (remove-if #'(lambda (n)

					(= 0 (mod n prime)))

				    (rest list))))))



(defun eratosthenes (max-num)

  (let ((all-numbers (loop for i from 2 to max-num collect i)))

    (sieve (first all-numbers) (rest all-numbers))))

```



With this change, `(biggest-cycle 1000 1000)` yielded `499` (in _211 seconds_), but it turned out to be incorrect. `(biggest-cycle 1000 10000)` took _22810 seconds_, which is shameful, but yielded the right answer.



### Update



Here is an effort to redeem myself:



```haskell

import qualified Data.List as L

import qualified Data.Vector as V



eratosthenes :: Int -> [Int]

-- ^Get a list of prime numbers less than the given number.                

eratosthenes n =

  let x = [2 .. n] in

  sieve (head x) (tail x)



sieve :: Int -> [Int] -> [Int]

sieve p nums =

  let rest = filter (\x -> x `mod` p > 0) nums

  in if null rest

     then [p]

     else p : sieve (head rest) (tail rest)



inverse :: Int -> Int -> V.Vector Int        

-- ^Given a number and the required precision, calculate the decimal expansion of its reciprocal.

inverse n prec =

  let truncate x y = (x `div` y, x `mod` y)

      nextDigit x count

        | count == 0 = []

        | otherwise = let (d,m) = truncate (x * 10) n

                      in d : nextDigit m (count - 1)

  in

   V.fromList $ nextDigit 1 prec



getInverses :: Int -> Int -> [V.Vector Int]

-- ^Get a list of all the inverses of all numbers upto a given number, for a given precision

getInverses maxNum maxPrec = [inverse x maxPrec | x <- [1 .. maxNum]]



checkRepeatLength :: V.Vector Int -> Int -> Int -> Int

checkRepeatLength expansion end len =

  let s1 = V.slice (end - len) len expansion

      s2 = V.slice (end - len - len) len expansion

      c = V.zipWith (==) s1 s2

  in

   if V.and c

   then len

   else 0



checkCycles :: V.Vector Int -> Int -> Int

-- ^Check if a cycle exists at a given end point with a given length; if it does, returns the length itself, otherwise 0

checkCycles expansion end =

  let l = quot end 2

      cycles = filter (> 0) $ map (checkRepeatLength expansion end) [1 .. l]

  in

   case cycles of

    x:xs -> x

    otherwise -> 0



findCycleLength :: V.Vector Int -> Int

-- ^Get the length of a cycle at a certain end point; if none found, returns 0

findCycleLength expansion =

  let l = V.length expansion

      minEnd = quot l 2

      cycles = filter (> 0) $ map (checkCycles expansion) [l, l-1 .. minEnd]

  in

   case cycles of

    x:xs -> x

    otherwise -> 0

   

euler26 :: Int -> Int

-- ^Attempts to solve Euler Problem #26 for a given precision

euler26 prec =

  let nums = [2 .. 1000]

      cycles = map (\x -> findCycleLength $ inverse x prec) nums

      maxPos = L.elemIndex (maximum cycles) cycles

  in

   case maxPos of

    Just n -> nums !! n

    _ -> 0

```



This gives the same answer (`euler26 10000`) in `7.5 seconds` (which is a 3000x speedup). Just to make sure you don't jump to that conclusion, it isn't a Haskell vs Lisp thing, it's just _calculating less stuff_, and doing it with _vectors instead of lists_.



In particular, the first version was running for _12 hours_ before I killed it out of disgust, and realized that instead of checking if the `length` of the list was greater than 0, I should do a `case` match instead since I only needed the first element. This made a _big_ difference. Also, this is the first time I _didn't_ have to rely on `Debug.Trace`, so I feel good about that :)
