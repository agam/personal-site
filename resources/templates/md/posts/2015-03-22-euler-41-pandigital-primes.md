{:title "euler 41 pandigital primes"
:layout :post
 :tags ["old-post"]}



Yet another _brute-force_ solution, pushing the line a bit at _1991 seconds_. But hey, it works, and was quick to write, so ...



**Statutory Warning**: _spoilers ahead_



```lisp

(defparameter *max-num-limit* 1000000000)

(defparameter *all-numbers* (make-array (list *max-num-limit*) :element-type 'bit :initial-element 1))



(defun mark-primes (&optional (n *max-num-limit*))

  ;; 0 and 1 are not prime

  (mark-not-prime 0)

  (mark-not-prime 1)

  ;; Mark 2 as prime, then do the following:

  ;; (1) Mark all multiples of the prime number,

  ;; (2) Find next available prime number, mark it as prime,

  ;; Repeat (1) until n

  (let ((prime 2))

    (loop while (< prime n) do

         (mark-prime-multiples prime n)

         (setf prime (find-next-prime prime n)))))



(defun mark-not-prime (idx)

  (setf (bit *all-numbers* idx) 0))



(defun mark-prime-multiples (prime limit)

  (do ((i (* prime 2) (+ i prime)))

      ((>= i limit))

    (mark-not-prime i)))



(defun find-next-prime (prev-prime limit)

  (do ((i (1+ prev-prime) (1+ i)))

      ((or (= i limit) (= 1 (bit *all-numbers* i))) i)))



(defun is-prime (n)

  (= 1 (bit *all-numbers* n)))



(defun get-num-digits (n)

  (ceiling (log n 10)))



(defun is-pandigital (n)

  (let* ((num-digits (get-num-digits n))

         (digits (make-array num-digits :element-type 'bit :initial-element 0)))

    (loop while (and (> n 0)

                     (> (mod n 10) 0)

                     (<= (mod n 10) num-digits)) do

         (setf (bit digits (1- (mod n 10))) 1)

         (setf n (floor (/ n 10))))

    (= (length digits) (count 1 digits))))



(defun euler41 ()

  (mark-primes)

  (do ((n 987654321 (1- n)))

      ((and (is-pandigital n) (is-prime n)) n)))

```



Everything is wrapped up in the call to `(euler41)`.
