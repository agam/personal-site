{:title "getting started with clojure part 1"
:layout :post
 :tags ["old-post"]}



(Just a personal log so I don't forget some basic steps here. There may or may not be a Part 2)



## Setting up the REPL



First, install relevant stuff. This is usually, at the minimum, something like `sudo apt-get install clojure1.4`



```

$ java -cp /usr/share/java/clojure-1.4.jar clojure.main

Clojure 1.4.0

user=> 

```



## First steps



Basic Arithmetic



```

user=> (+ 2 3)

5

```



Basic string operation



```

user=> "foo"

"foo"



user=> (concat "foo" "bar")

(\f \o \o \b \a \r)

```



Variables

```

user=> (def x 4)

#'user/x



user=> (+ x 5)

9



user=> (def strarr ["foo" "bar"])

#'user/strarr

```



Basic arrays



```

user=> (count [6 5 3 4 2])

5



user=> (get strarr 1)

"bar"



user=> (get strarr 0)

"foo"

```



Basic maps



```

user=> (def mymap {:foo "Here is foo" :bar "... and here is bar!"})

#'user/mymap



user=> (get mymap :bar)

"... and here is bar!"

```



Errors



```

user=> (^ 6 4)

IllegalArgumentException Metadata must be Symbol,Keyword,String or Map  clojure.lang.LispReader$MetaReader.invoke (LispReader.java:720)

4

RuntimeException Unmatched delimiter: )  clojure.lang.Util.runtimeException (Util.java:170)



user=> (% 6 4)

CompilerException java.lang.RuntimeException: Unable to resolve symbol: % in this context, compiling:(NO_SOURCE_PATH:2) 

```



Stack Overflow (!)



```

user=> (defn foo [x] (foo (- x 1)))

#'user/foo



user=> (foo 4)

StackOverflowError   user/foo (NO_SOURCE_FILE:7)

```



Simple function



```

user=> (defn expt [x y] (if (= y 0) 1 (* x (expt x (- y 1)))))

#'user/expt



user=> (expt 6 4)

1296

```



Default REPL namespace (`user`)



```

user=> (user/expt 6 4)

1296

```



## Programming primitives



For loop



```

user=> (dotimes [_ 5] (println "Hello World."))

Hello World.

Hello World.

Hello World.

Hello World.

Hello World.

nil

```



While loop



```

user=> (for [i (range 5) j (range 5) :when (and (> i 0) (< i j))] (println (* i j)))

(2

3

4

nil nil 6

8

nil nil 12

nil nil)

```



Switch case



```

user=> (for [i (range 5) j (range 5)]

(case (* i j)

0 "zero"

(* i j)))

("zero" "zero" "zero" "zero" "zero" "zero" 1 2 3 4 "zero" 2 4 6 8 "zero" 3 6 9 12 "zero" 4 8 12 16)

```



... the rest in Part 2, if there is one.
