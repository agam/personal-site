{:title "euler 25 fibonacci digits"
:layout :post
 :tags ["old-post"]}



**Statutory Warning**: spoilers ahead



_Problem: find the first fibonacci number with 1000 digits_



First though: hah, I can get the Nth fibonacci number in O(1) time!



```lisp

(defun fib (n)

  (let ((inv-sq-5 (/ 1 (sqrt 5)))

	(phi (/ (1+ (sqrt 5)) 2)))

    (floor (+ 0.5 (* inv-sq-5 (expt phi n))))))



(defun digits (n)

  (round (1+ (log n 10))))



(defun log-fib-10 (n)

  (let ((log-10 (log 10))

	(log-phi (log (/ (1+ (sqrt 5)) 2)))

	(log-k (log (/ 1 (sqrt 5)))))

    (* log-10 (+ log-k (* n log-phi)))))

```



Nope, floating point numbers don't go beyond 308 digits or so



```lisp

CL-USER> most-positive-double-float

1.7976931348623157d308

```



Perhaps there was a clever way to workaround this, but I wasn't feeling clever enough at the time. So, back to brute force.



```lisp

(defun fib (n)

  (cond ((= n 0) 0)

	((= n 1) 1)

	(t (+ (fib (- n 1)) (fib (- n 2))))))

```



Well, this just takes _too long_ (`(fib 40)` takes `3.883` seconds, and that has just 9 digits, so forget about anything bigger). Next step: memoization.



```lisp

(let ((memo-table (make-hash-table)))

  (defun memo-fib (n)

    (multiple-value-bind (hash-val exists) (gethash n memo-table)

      (if exists

	  hash-val

	  (let ((new-val (cond ((= n 0) 0)

			               ((= n 1) 1)

			               (t (+ (memo-fib (- n 1)) (memo-fib (- n 2)))))))

	    (setf (gethash n memo-table) new-val)

	    new-val)))))



(defun digits-num (n)

  (length (princ-to-string n)))

```



(yeah just to be even less clever, I get the digits by seeing the length of the string representation of the number ... works)



`(memo-fib 40)` is instantaneous, so is `(memo-fib 1000)`, and `(memo-fib 10000)` takes `3 ms`, so this should do it.



I used this to manually guess my way around and found the answer. Woohoo!



But this last part felt a bit ... dirty, so before quitting this problem, I decided to automate it this way:



```lisp

(defun bin-search-range (fn target)

  (do* ((i 1 (* 2 i))

       (val (funcall fn 1) (funcall fn i)))

       ((>= val target) (values i (/ i 2)))

    (format t "Debug: Trying (~A, ~A)~%" i val)))



(defun bin-search (fn low high target)

  (let* ((mid (round (/ (+ low high) 2)))

	 (mid-val (funcall fn mid)))

    (format t "Debug: Searching b/w (~A, ~A), found (~A, ~A)~%" low high mid mid-val)

    (cond ((= high mid) mid)

	  ((>= mid-val target) (bin-search fn low mid target))

	  ((< mid-val target) (bin-search fn mid high target)))))



(defun solve (fn target)

  "Two-phase search; first, exponentially increase argument until target is exceeded, then begin binary search with last argument"

  (multiple-value-bind (high low) (bin-search-range fn target)

    (bin-search fn low high target)))



(defun target-function (n)

  (digits-num (memo-fib n)))



(defun euler-25 ()

  (solve #'target-function 1000))

```



Yes, it does work. I left in my original debug statements (BTW the initial version had not one but _two_ off-by-one errors! I learn slowly ...), so the working is illustrated as follows:



```lisp

CL-USER> (euler-25)

Debug: Trying (1, 1)

Debug: Trying (2, 1)

Debug: Trying (4, 1)

Debug: Trying (8, 2)

Debug: Trying (16, 3)

Debug: Trying (32, 7)

Debug: Trying (64, 14)

Debug: Trying (128, 27)

Debug: Trying (256, 54)

Debug: Trying (512, 107)

Debug: Trying (1024, 214)

Debug: Trying (2048, 428)

Debug: Trying (4096, 856)

Debug: Searching b/w (4096, 8192), found (6144, 1284)

Debug: Searching b/w (4096, 6144), found (5120, 1070)

Debug: Searching b/w (4096, 5120), found (4608, 963)

Debug: Searching b/w (4608, 5120), found (4864, 1017)

Debug: Searching b/w (4608, 4864), found (4736, 990)

Debug: Searching b/w (4736, 4864), found (4800, 1003)

Debug: Searching b/w (4736, 4800), found (4768, 997)

Debug: Searching b/w (4768, 4800), found (4784, 1000)

Debug: Searching b/w (4768, 4784), found (4776, 998)

Debug: Searching b/w (4776, 4784), found (4780, 999)

Debug: Searching b/w (4780, 4784), found (4782, 1000)

Debug: Searching b/w (4780, 4782), found (4781, 999)

Debug: Searching b/w (4781, 4782), found (4782, 1000)

4782

```



The whole thing took `6 ms`. Not bad, eh?
