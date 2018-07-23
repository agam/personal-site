{:title "euler 22 scoring names"
:layout :post
 :tags ["old-post"]}



This one seemed simple but turned out to be a good showcase for a short haskell program (I'm sure it can be shorter but IMHO that would probably approach _Perl-ness_)



```haskell

import qualified Data.Char as C

import qualified Data.List as L

import qualified Debug.Trace as T



charValue c = C.ord (C.toUpper c) - C.ord 'A' + 1



nameValue n = sum $ map charValue n



nameProduct (name, index) = index * nameValue name



splitString c s =

  case break (== c) s of

       ([], _:xs) -> [xs]

       (y, _:xs) -> [y] ++ splitString c xs

       (xs, []) -> [xs]



getNameSum f =

     let w = splitString ',' $ filter (/= '\"') f

     in

        T.traceShow (length w)

        sum $ map nameProduct $ zip (L.sort w) [1 .. ]



getNames fileName =

  do

     f <- readFile fileName

     return $ getNameSum f

```



I left the `traceShow' in there to give an example of the debugging I relied upon. Other than that, it was enjoyable, and I really do appreciate how much longer this would have been in most other languages.



**Notes:**



- I skipped the part about _sorting_ the input and spent a long time (_unnecessarily!_) complaining about the _black-boxness_ of the "IO"



- I suspected the initial version had an off-by-one error; luckily `awk` was on hand to sanity check the number of words:



```sh

$ cat /tmp/p022_names.txt | tr ',' ' ' | tr '"' ' ' | tr -s " " | awk 'BEGIN { RS = " " }; END { print NR }'

5163

```



- Just to dump it out there, the _other_ way of checking the soundness of the logic:



```lisp

(defun char-value (c)

  (1+ (- (char-code c)

	 (char-code #\A))))



(defun string-value (str)

  (loop for c across (string-upcase str)

       sum (char-value c)))



(defun name-product (name index)

  (* index

     (string-value name)))



(defun range (n)

  (loop for i from 1 to n

     collect i))



(defun sum-names (names)

  (reduce #'+

	  (mapcar #'name-product

		  names

		  (range (length names)))))

```



... used (e.g.) as `(sum-names (list "foo" "bar"))`



- For some reason, the code-formatter for Jekyll (`pygments`) is completely thrown off by the Haskell source above. Perhaps this post have been in "literate code". Most likely it would have been procrastinated out of existence if attempted that way.
