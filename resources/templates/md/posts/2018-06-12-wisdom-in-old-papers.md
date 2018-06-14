{:title "On the wisdom in old papers"
:layout :post
:tags ["papers"]}


At the risk of over-generalizing, recent papers in computer science are _highly topical_, with lots of domain-specific verbiage. On the other hand, a lot of older papers seem to be full of little nuggets of “deep insight” just strewn around.

As an example, I happened to come across [a paper] by Dahl (of _Simula_ fame) and Hoare (of _CSP_ fame) which, on the surface, is talking about **Simula 67** and **Algol 60**, and might come across as _boring_ (or worse, _old-fashioned_!).

Instead, it is full of stuff like this:

> In short, data and operations on data seem to be so closely connected in our minds, that **it takes elements of both kinds to make up any concept useful for understanding computing processes**.

> … we have to deal with computing processes of such complexity that they can hardly be constructed and understood in terms of basic general purpose concepts. The limit is set by the nature of our own intellect: **precise thinking is possible only in terms of a small number of elements at a time**.

> The only efficient way to deal with complicated systems is in a hierarchical fashion. The dynamic system is constructed and understood in terms of high level concepts, which are in turn constructed and understood in terms of lower level concepts, and so forth. This must be reflected in the structure of the program which defines the dynamic system; **in some way or another the higher level concepts will correspond to program components**.


I found a summary paper that referenced this paper, that had this interesting slide:

> Type Abstraction ≠ Procedural Abstraction

> CLU provides ADTs: fundamentally different from objects!

> Did Liskov and the CLU team realize this?

> Simula’s class construct can be used to generate both records (unprotected, or protected by type abstraction) and objects (protected by procedural abstraction)

> C++ can also be used to program data abstractions as well as objects

It goes on and on and on ...

