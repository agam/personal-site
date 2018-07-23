{:title "some lisp exercises"
:layout :post
 :tags ["old-post"]}



Reading through ANSI Common Lisp, and doing some of the exercises as I come across them. Here are a few from Ch 4 and 5:



```cl

(defun rotate-square-array (arr)

  (let* ((dim (car (array-dimensions arr)))

	(newarr (make-array (list dim dim))))

    (dotimes (i dim)

      (dotimes (j dim)

	(setf (aref newarr j (- dim (1+ i))) (aref arr i j))))

    newarr))



(defun rev-l (lst)

  (reduce (lambda (elem acc) (cons acc elem)) lst :initial-value nil))



(defun copy-l (lst)

  (reduce (lambda (elem acc) (cons elem acc)) lst

	  :initial-value nil :from-end t))



(defun alist-to-ht (alist)

  (let ((ht (make-hash-table)))

    (mapcar (lambda (kvpair)

	      (setf (gethash (car kvpair) ht) (cdr kvpair)))

	    alist)

    ht))



(defun ht-to-alist (ht)

  (let ((alist '()))

    (maphash (lambda (k v)

	       (setf alist (acons k v alist)))

	     ht)

    alist))



(defun precedes-iter (obj vec)

  (let ((pos-list '()))

    (do* ((last-pos (position obj vec)

		    (position obj vec :start (1+ last-pos))))

	 ((eql last-pos nil) pos-list)

      (and (> last-pos 0)

	   (let ((elem (elt vec (- last-pos 1))))

	     (setf pos-list (adjoin elem pos-list)))))))





(defun precedes-recur (obj vec)

  (labels ((precedes-helper (obj vec pos-list start-pos)

	     (let ((next-pos (position obj vec :start start-pos)))

	       (if (null next-pos)

		   pos-list

		   (precedes-helper obj vec

				    (adjoin (elt vec (- next-pos 1))

					    pos-list)

				    (1+ next-pos))))))

    (precedes-helper obj vec '() 1)))





(defun intersperse-recur (obj lst)

  (labels ((intersperse-helper (obj lst)

	     (if (null lst)

		 nil

		 (cons obj (cons (car lst)

				 (intersperse-helper obj (cdr lst)))))))

    (cons (car lst) (intersperse-helper obj (cdr lst)))))



(defun intersperse-iter (obj lst)

  (do ((iter-lst (cdr lst) (cdr iter-lst))

       (result (list (car lst)) (cons (car iter-lst) (cons obj result))))

      ((null iter-lst)

       (reverse result))))



(defun within-one (x y)

  (or (= x (1+ y))

      (= y (1+ x))))



(defun orderedlistp-recur (lst)

  (labels ((ordered-list-helper (elem lst)

	     (if (null lst)

		 t

		 (let ((x (car lst)))

		   (and (within-one elem x)

			(ordered-list-helper x (cdr lst)))))))

    (ordered-list-helper (car lst) (cdr lst))))





(defun orderedlistp-iter (lst)

  (do* ((ordered t

		 (within-one first-elem (car remaining)))

	(first-elem (car lst)

		    (car remaining))

	(remaining (cdr lst)

		   (cdr remaining)))

       ((or (null remaining)

	    (not ordered))

	ordered)))



(defun orderedlistp-map (lst)

  (flet ((ordered-list-mapper (elem1 elem2)

	   (if (not (within-one elem1 elem2))

	       (return-from orderedlistp-map nil))))

  (mapc #'ordered-list-mapper lst (cdr lst))

  t))



(defun max-and-min (lst)

  (labels ((max-min-helper (lst curmin curmax)

	     (if (null lst)

		 (values curmin curmax)

		 (let ((elem (car lst))

		       (rest (cdr lst)))

		   (cond

		     ((and (eql curmin nil)

			   (eql curmax nil))

		      (max-min-helper rest elem elem))

		     ((> elem curmax)

		      (max-min-helper rest curmin elem))

		     ((< elem curmin)

		      (max-min-helper rest elem curmax))

		     (t

		      (max-min-helper rest curmin curmax)))))))

    (max-min-helper lst nil nil)))

```
