{:title "euler 42 coded triangle numbers"
:layout :post
 :tags ["old-post"]}



Very straightforward, this one. And I don't need to feel bad that I did it the simple way. If you're wondering where `split-sequence-if` and `read-file` came from, the answer is `rutils`, which is a good general-purpose utility package, a sort of superset of `alexandria`, and just a `(ql:quickload 'rutils)` away. As before, the final answer is obtained by evaluating `(euler42)`.



**Statutory Warning**: _spoilers ahead_



```lisp

;; If x = 0.5 * n * (n+1) is a triangle number, then there is an

;; integral solution for n^2 + n - 2*x = 0.

(defun quadratic-solution (a b c)

  (let ((det (sqrt (- (* b b) (* 4 a c))))

        (denom (* 2 a)))

    (cons (/ (- (- b) det) denom)

          (/ (+ (- b) det) denom))))



(defun positive-int (num)

  (and (integerp num)

       (plusp num)))



(defun is-triangle-number (num)

  (let ((soln (quadratic-solution 1 1 (* (- 2) num))))

    (or (positive-int (car soln))

        (positive-int (cdr soln)))))



(defun char->number (char)

  (1+ (- (char-code char) (char-code #\A))))



(defun is-triangle-word (word)

  (let ((value (loop for char across word

                  summing (char->number char))))

    (is-triangle-number value)))       

  

(defun euler42 ()

  ;; Read in the file, split on the commas, remove the quotes

  (let* ((file-text (read-file "/home/agam/Downloads/p042_words.txt"))

         (words-with-quotes (split-sequence-if

                             (lambda (char) (eq char #\,))

                             file-text))

         (words (mapcar (lambda (w) (remove #\" w))

                        words-with-quotes)))

    (loop for w in words count

         (is-triangle-word w))))

```


