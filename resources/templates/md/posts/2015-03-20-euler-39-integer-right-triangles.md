{:title "euler 39 integer right triangles"
:layout :post
 :tags ["old-post"]}



Straightfoward, this. An interesting insight into speed difference between `SBCL` and `Clozure`: the former took _0.75 seconds_ to run this, while the latter took about _4 seconds_ (without the type annotations, `SBCL` takes _33 seconds_).



```lisp

(defun is-right-triangle (n1 n2 n3)

  (declare (type fixnum n1 n2 n3))

  (= (+ (expt n1 2) (expt n2 2)) (expt n3 2)))



(defun triangle-solutions (n)

  (declare (type fixnum n))

  ;; Largest side (hyp) can't be less than a third of the total

  (loop for hyp from (floor (/ n 3)) upto (- n 2)

     ;; Avoid repeating combinations, so WLOG let one side

     ;; be greater than the other

     for sides-sum = (- n hyp)

     when (loop for side1 from (ceiling (/ sides-sum 2)) to (1- sides-sum)

             for side2 = (- sides-sum side1) 

             when (is-right-triangle side1 side2 hyp)

             collect (list side1 side2 hyp))

     collect it))



(defun max-triangle-solutions (n)

  (declare (type fixnum n))

  ;; n cannot be less than 3

  (let* ((solutions (loop for i from 3 to n

                       collect (length (triangle-solutions i))))

         (max-solution (loop for elem in solutions maximizing elem)))

    (cons max-solution (+ 3 (position max-solution solutions)))))

```



The answer needed is given by `(max-triangle-solutions 1000)`, while any specific solution can be got by (e.g.) running `(triangle-solutions 120)` which yields `(((40 30 50)) ((45 24 51)) ((48 20 52)))`


