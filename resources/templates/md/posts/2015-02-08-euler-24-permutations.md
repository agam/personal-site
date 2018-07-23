{:title "euler 24 permutations"
:layout :post
 :tags ["old-post"]}



(The fact that this turned out to be a long story is _ridiculous_, but perhaps it'll be useful to someone else)



The idea here is intuitive: given say `0`, `1` and `2`, we can immediately come up with the following orderings:



```

012

021

102

120

201

210

```



The problem here is to find the millionth permutation of `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`.



The first way is to _cheat_ and work it out with pen-and-paper, upon which you soon realize that there are cycles for each set of numbers. E.g. for two numbers we would have 2 possible orderings (`01` and `10`), for three we have the ones shown above, etc, and this is because for `n` numbers we have `n!` combinations. Right, that's obvious. But the next step is to see that _within_ each block of `n!`, there are `n` blocks of size `(n-1)!`, where the first digit is the same, which means we have a straightforward way of reducing the problem of size `n` to a problem of size `n-1`.



As an example, to find the fifth permutation in the example above, we would see that it involves two cycles of two digits each, and then halfway through a single cycle. So we can come up with the first digit being `2`, and the next digit being the second of the remaining digits (i.e. `1`), and we're finally left with `0`.



(**Note**: Argh ... This sentence is wrong! But I didn't realize that until later)



So I wrote the code for this, and I used Ocaml because I'm a complete n00b at it.



```ocaml

let rec fact n =

  if n = 0 then 1

  else n * fact (n - 1)



let perm_total = 1_000_000;;



let get_perm total index =

  let fact_index = fact index in

  total / (fact_index), total mod fact_index



let rec get_all_perms total index_size =

  let rec get_next_perm total index_size perm_list =

    if index_size = 0 then perm_list

    else match get_perm total index_size with

	 | p, new_total -> get_next_perm new_total (index_size - 1) (p :: perm_list)

  in List.rev (get_next_perm total index_size [])



let rec digits_list n lst =

  if n = 0 then (0 :: lst)

  else digits_list (n - 1) (n :: lst)



let remove_digit digit lst =

  List.filter (fun x -> x <> digit) lst



let rec get_next_perm_digit perm_list digit_list pdlist =

  if List.length perm_list = 0 then pdlist

  else let p = List.hd perm_list in

       let d = List.nth digit_list p in

       get_next_perm_digit (List.tl perm_list) (remove_digit d digit_list) (d :: pdlist)

	      

let rec get_perm_digits perm_list =

  let digits = digits_list (List.length perm_list) [] in

  List.rev (get_next_perm_digit perm_list digits [])



let euler24 = get_perm_digits (get_all_perms perm_total 9);;	   

```



Yes, the names are terrible. Many of the started out as nested functions which I pulled into the top-level to test separately. Anyway, the point is ... the answer was **wrong**.



Ok, I thought, I must've screwed up in the _Ocaml-ness_ of my solution. So I rewrote it thusly:



```lisp

(defun fact (n)

  (if (= n 1)

      1

      (* n (fact (1- n)))))



(defparameter *total* 1000000)



(defun get-perm (total index)

  (let ((f (fact index)))

    (truncate total f)))



(defun get-next-perm (total index-size perm-list)

  (if (= index-size 0)

      (cons 0 perm-list)

      (multiple-value-bind (p new_total)

	  (get-perm total index-size)

	(get-next-perm new_total (1- index-size) (cons p perm-list)))))



(defun get-all-perms (total index-size)

  (reverse (get-next-perm total index-size '())))



(defun digits-list (n)

  (loop for i from 0 to n

       collect i))



(defun get-next-perm-digit (perm-list digit-list p-d-list)

  (if (null perm-list)

      p-d-list

      (let* ((p (first perm-list))

	     (d (nth p digit-list)))

	(get-next-perm-digit (rest perm-list)

			     (remove d digit-list)

			     (cons d p-d-list)))))



(defun get-perm-digits (perm-list)

  (let ((digits (digits-list (1- (length perm-list)))))

    (reverse (get-next-perm-digit perm-list digits '()))))



(defun euler-24 ()

  (get-perm-digits (get-all-perms *total* 9)))

```



The _same_ answer popped out -- which means I was successful at my translation -- but it was still the wrong answer. In the initial few seconds of denial, I refreshed the Project Euler page and tried again. No luck.



Now it was time for desperate measures, so I came up with this brute force solution:



```lisp

(defun list->number (list)

  (reduce (lambda (x y) (+ (* x 10) y)) list))



(defun number->list (n)

  (do ((tempn n (floor (/ tempn 10)))

       (digits '() (cons (mod tempn 10) digits)))

      ((= tempn 0) digits)))



(defun has-digits (n digit-list)

  (let ((nlist (number->list n)))

    (not (set-difference digit-list nlist))))



(defun brute-force-next-permutation (digit-list)

  (let ((n (list->number digit-list)))

    (do ((trynum (1+ n) (1+ trynum)))

	((has-digits trynum digit-list) (number->list trynum))

      (format t "Trying ~A~%" trynum))))

```



.... which wasn't _quite_ right either, since it skipped the leading zero in our lists. This one seemed to work:



```lisp



(defun add-num (digit-list)

  (let ((sum (1+ (first digit-list))))

    (if (< sum 10)

	(cons sum (rest digit-list))

	(cons (mod sum 10) (add-num (rest digit-list))))))



(defun next-number (digit-list)

  (let ((revlist (reverse digit-list)))

    (nreverse (add-num revlist))))



(defun brute-force-next-permutation (digit-list)

  (do ((trynum (next-number digit-list) (next-number trynum)))

      ((not (set-difference digit-list trynum)) trynum)))



(defun brute-force-nth-permutation (digit-list n)

  (do ((i 1 (1+ i))

       (trynum digit-list (brute-force-next-permutation trynum)))

      ((= i n) trynum)))

```



So I set that running with `(brute-force-nth-permutation '(0 1 2 3 4 5 6 7 8 9) 1000000)`, and it looked like it was clearly going to take a while.



At this point I was really depressed, since I couldn't figure out _what_ was wrong with the initial approach which was so _straightforward_ ... and then I realized that **I was probably off by one**.



So I took the answer submitted earlier, and entered the _next_ permutation. Nope. Ok, what about the _previous_ one? That worked!



(**Note**: To circle back to the 'trivial' example at the top of the post: I didn't catch myself talking about the fifth permutation as being 0,1,2,3,4,5 -- so I was really talking about the _sixth_ permutation ...)



Later, the (inefficient) brute-force computation terminated in `1668` seconds, and happily, the answers matched. I tried the previous code with a small modification:



```lisp

(defun euler-24 ()

  (get-perm-digits (get-all-perms (1- *total*) 9)))

```



... and it gave the same answer (and obviously, so did the Ocaml version). So a happy ending to this story, but a painful reminder of the fact that **there are only two hard problems in computer science: naming, caching, and off-by-one errors.**
