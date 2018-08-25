{:title "Clojure's incredibly frustrating lack of debugging"
 :layout :post
 :tags ["clojure"]}


I thought it might be fun to follow along [PAIP](https://github.com/norvig/paip-lisp) and translate the code to [Clojure](https://bitbucket.org/agambrahma/paip-clj/src/master/).

But almost as soon as I started, I found myself giving up. Debugging is just too _hard_.

Not hard compared to regular compiled languages, or even Python, etc. I mean, there _is a stacktrace_.

It’s hard compared to Common Lisp, where I don’t just see a stacktrace, I see each frame, I see all the state at each frame, I can step back and forth with ease. 

It’s a shame, and maybe I’ll find a different excuse some day to dive back in, since _Spec_ and _Datomic_ (especially with the new _Ions_) are exciting new things, but … unless the tooling takes a _giant_ step forward, it’s just not worth it.
