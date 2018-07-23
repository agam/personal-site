{:title "ridiculous concurrency"
:layout :post
 :tags ["old-post"]}



Ok, so I went through the [Go Tour](http://tour.golang.org/), and it's pretty impressive; having this sort of super-fast edit-compile-run cycle is something usually associated with Javascript-ish languages, certainly not a statically compiled systems language.



One of the big selling points of Go (apart from its "familiar" yet clean syntax) is the ubiquitous concurrency primitives available. Yes, you could obviously do the same in other languages (see a good [Clojure translation here](http://blog.drewolson.org/blog/2013/07/04/clojure-core-dot-async-and-go-a-code-comparison/)), but channels and goroutines being first-class objects in Go, passing them is as normal as ints and strings.



Except its not really that normal, is it? But it _should_ be. So I thought, why not try out something that I called "ridiculous concurrency", a sort of roundabout way of doing normal stuff, while _overusing_ concurrency.



This isn't meant to be a good way of showing how to do anything, but rather **making it easy to _abuse_ concurrency, so that you can later _use_ it**.


