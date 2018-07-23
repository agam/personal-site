{:title "static typing"
:layout :post
 :tags ["old-post"]}



Numerous people smarter than me have weighed into the static vs dynamic typing debate, so I'll be brief: I won't pick sides because _there are no sides to pick_. Except for some _obvious_ cases where there is little disagreement, the word "type" just happens to mean two very different things when putting forth arguments on the benefits of either _side_.



Looking back now, the only language that I've seen where the distinction I'm talking about is made explicit, is SML. The `type` keyword is _different_ from the `datatype` keyword. The benefits of static types are tied up with the former, whereas people defending dynamic typing are usually resisting an _overuse_ of the latter (with a mass of confusion in the middle).



The usual way (C, C++, Java) of thinking about types is "struct" types or "class" types, i.e. some sort of _storage_ type for values -- given some raw bits, what can I "cast" these bits to? What _datatype_ do these bits represent?



From this perspective, having some guarantees about the datatype represented by these bits is **very important**, and this is the reason given by people preferring (say) C++/Java over either untyped or weakly typed languages (e.g. C, Python).



One outlier worth mentioning is the case where the primitive level is _not_ raw bits, but run-time-typed values, as in Lisp. Here the low-level stuff _does_ have a type, so you don't _really_ need the strong guarantees of "which datatype do these bits represent?" (and therefore, this is where the static vs dynamic typing debate really heats up).



On the other hand, _types_ as in ML/Haskell etc. are a superset (usually) of datatypes, and might be better thought of as "tags" of some sort -- tags that satisfy certain constraints. Roughly, if you tag your values correctly, then the implicit constraints of the **type system** will guarantee constraints on how these values might be related, which in turn implicitly proves certain invariants of your program.



As an example, statically typed programs in these languages heavily use _Algebraic Data Types_, which are practically absent from both dynamically-typed languages (e.g. Lisp) as well as languages that stick to strong datatypes (e.g. C++). This makes sense -- you don't "cast" a bit pattern into an algebraic data type; you use it to constrain the uses of values tagged with it.



Anyway, there are _real_ tradeoffs in the static-vs-dynamic debate, but a very small and explicit set. Most of the heat is just people talking past each other, using and misusing this overloaded word. Perhaps it would be better if we could just be disciplined enough to distinguish between `type` and `datatype` where it matters.
