{:title "euler 42 pandigitals with sub string divisibility"
:layout :post
 :tags ["old-post"]}



These brute force solutions are getting a bit worrying, but here is another one. (I keep promising myself to get out of my comfort zone, but (_unfortunately!_) code like this is too easy to write).



**Statutory Warning**: _Spoilers ahead_



```lisp

;; For all 0-9 pandigital numbers, find the ones which have successive

;; substrings of length 3 diisible by 2,3,...,17.



(defparameter *prime-divisors* #(17 13 11 7 5 3 2))



;; A number is 0-9 pandigital if it (a) is 10 digits long, and (b) has every digit from 0-9

(defun is-pandigital (num)

  (declare (type fixnum num))

  (let ((digits-seen (make-array 10 :element-type 'bit :initial-element 0)))

    (do* ((n num (floor (/ n 10)))

          (d (mod n 10) (mod n 10))

          (num-digits 0 (1+ num-digits)))

         ((= n 0) (and (= num-digits 10)

                       (every (lambda (x) (= x 1)) digits-seen)))

      (setf (bit digits-seen d) 1))))



(defun check-divisibility (num div-index)

  (= 0 (mod num (aref *prime-divisors* div-index))))



(defun get-three-digit-num (num)

  (let ((ones (mod num 10))

        (tens (mod (floor (/ num 10)) 10))

        (hundreds (mod (floor (/ num 100)) 10)))

    (+ ones

       (* 10 tens)

       (* 100 hundreds))))



;; Go backwards in groups of three digits and check divisibility

(defun has-divisible-substrings (num)

  (do* ((n num (floor (/ n 10)))

        (div-index 0 (1+ div-index))

        (dividend (get-three-digit-num n) (get-three-digit-num n))

        (div-p (check-divisibility dividend div-index)

               (and div-p (check-divisibility dividend div-index))))

       ((< n 10000) div-p)))



(defun check-substring-pandigital-range (start end)

  (declare (type fixnum start end))

  (let ((candidates '()))

    (loop for num from start to end do

         (if (and (is-pandigital num)

                  (has-divisible-substrings num))

             (push num candidates)))

    candidates))



(defun euler43 ()

  (let ((candidates (check-substring-pandigital-range 1234567890 9876543210)))

    (print candidates)

    (apply #'+ candidates)))

```



As before, evaluating `(euler43)` shows the final solution (sum), along with the (six!) candidate numbers making up the sum. This took a whopping `4.3 hours` to churn through the 8.5 billion numbers. Maybe I need to create a new constraint for these problems: either pen-and-paper, or something _slow_ (like Python ?! :P), so that brute force is never tempting again.


