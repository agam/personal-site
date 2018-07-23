{:title "a taste of j"
:layout :post
 :tags ["old-post"]}



I recently stumbled across APL and J, as mentioned in a previous post, and began reading a bit of [an introductory book](http://www.jsoftware.com/help/learning/contents.htm).



A couple of days ago, I came across this series of coding tasks on a blog called "Programming Praxis", and I picked [the most recent one](http://programmingpraxis.com/2015/10/06/the-iron-bar/) as an example to try out some J noobism.



The example is simple but contrived (and _median_ is a terrible name) -- we basically have to write a function that that takes two arguments and either increments, decrements or does nothing, with the first argument. In J, like so many other tasks, this is essentially one line (12 characters, to be precise).



```

median =: [ + (* @: -)~

```



To give some idea on what's going on here, this expression can be broken, first, into three parts: `[`, `+`, `(* @: -)~`. Let's start with the sub-expression within the parentheses: `* @: -`.



Here, `*` is a _verb_ (think: _"function"_) that returns 1, 0, or -1 depending on the sign of its argument. So `* 4` is `1`, `* _2` (that's how -2 is represented) is `_1`, `* 0` is `0`.



`-` is the only thing that does what you would expect: `4 - 3` is `1`, and so on.



`@:` is a _conjunction_, such that `f @: g` when applied to _x_ has the effect of _f (g x)_.



`]` is a verb that when given two values, always selects the first of the two.



`~` is an _adverb_ that reverses the order of arguments to its preceding verb. So while `%` applied to _x_ and _y_ yields _(x % y)_, `%~` applied to _x_ and _y_ yields _(y % x)_.



Now things get a little tricky (but only a _little_; we're barely scratching the surface of J here). The expression for median can be seen as three verbs laid down together, and this is interpreted as a _"train"_ of verbs, or more specifically in this case, a _"dyadic fork"_.



Basically, _(f g h)_ when applied to two values _x_ and _y_, is equivalent to _g_ applied to the result of _f_ applied to _x_ and _y_, and the result of _h_ applied to _x_ and _y_. Or, using infix notation,



```

x (f g h) y   is equivalent to    (x f y) g (x h y)

```



The reason _x_ and _y_ aren't present in the definition of _median_ is that it's defined using a "tacit" form rather than an "explicit" form (somewhat similar to "point-free" definitions in Haskell).



Putting it all together now, when applied to two arguments, we get the equivalent of `(x [ y) + (* (y - x))`, which gives `x + 1` when _y > x_, `x + 0` when _y == x_, and `x + (-1)` when _y < x_, which is what we want.



If you like this sort of thing, head over to [jsoftware.com](http://www.jsoftware.com/) for more.
