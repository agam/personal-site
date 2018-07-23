{:title "euler 40 champernownes constant"
:layout :post
 :tags ["old-post"]}



I was too lazy to avoid the brute-force approach here, especially since it took less than a second to run. The following seems longer than it should be, partly because I left in some helper and debugging functions ...



_Edit_: There is obviously a very nice paper-and-pen solution to this -- the 10th place is '1' (for 10), then the 100th place is '5' (for 55), and so on ...



**Statutory Warning**: _Spoilers Ahead_



```lisp

(defparameter *max-digits* 2000000)



(defun digits (n)

  (declare (type fixnum n))

  (nreverse (loop

               for tmp = n then (floor (/ tmp 10))

               until (= tmp 0)

               collect (mod tmp 10))))



(defun set-digits (d start all-digits)

  (loop

     for elem in d

     for idx = start then (1+ idx) do

       (setf (aref all-digits idx) elem)))



;; Fills out the array of digits and returns an accessor function

;; Note: the index increments by the length of the number of

;; digits of the _previous_ number.

(let ((all-digits (make-array (list *max-digits*) :element-type '(integer 0 9) :initial-element 0)))

  (defun populate-digits (n)

    (progn

      (loop

         for num = 1 then (1+ num)

         for d = (digits num)

         for i = 0 then (+ i (length (digits (1- num))))

         while (< i n) do

           (set-digits d i all-digits))

    (lambda (idx)

      (aref all-digits idx)))))



(defun power-list ()

  (let ((champer (populate-digits 1000000)))

    (loop for p from 0 to 6 collect

         (funcall champer (1- (expt 10 p))))))



(defun euler40 ()

  (apply '* (power-list)))





;; Useful debugging tool

(defun scan-list (start end)

  (let ((champer (populate-digits)))

    (loop for i from start to end do

         (print (cons i (funcall champer i))))))

```



The "final answer" is given by `(euler40)`, and the intermediate digits themselves by `(power-list)`. I found `(scan-list)` useful to debug an embarassing off-by-one error in the loop.
