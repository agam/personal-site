{:title "minsky turing lecture part 1"
:layout :post
 :tags ["old-post"]}



I came across a reference to Marvin Minsky's 1970 Turing Award Lecture. I've barely read any of these lectures, always assuming they would be empty or pedantic or plain boring. Reading this changed that perception completely.



## Science and Education

The structure of this lecture seems very interesting to me; it seems to be divided into two parts. The first focussing more on computer science and its failings, and the second on (what seems to me) education.



This post is about the first part. And by "about" I mean just extracts with minimal commentary.





## The trouble with Computer Science ...



> The trouble with computer science today is an obsessive concern with form instead of content.



(yes, that's the opening line!)



> This essay has three parts, suggesting form-content confusion in _theory of computation_, in _programming languages_, and in _education_.



(yes, I said two parts earlier ... I just clubbed the first two together)



> Suppose that we had _n_ computers instead of just one. How much can we speed up what kinds of calculations? For some, we can surely gain a factor of _n_. But these are rare. For others, we can gain log _n_, but it is hard to find any or to prove what are their properties. And for most, I think, we can gain hardly anything; this is the case in which there are many highly branched conditionals, so that look-ahead on possible branches will usually be wasted. **We know almost nothing about this; most people think, with surely incorrect optimism, that parallelism is usually a profitable way to speed up most computations.**



("surely incorrect optimism"! I agree!)



> ... in today's computer science curricula ... almost all their time is devoted to formal classifications of syntactic language types, defeatist unsolvability theories, **folklore about systems programming**, and generally trivial fragments of "optimization of logic design" -- the latter often in situations where the art of heuristic programming has far outstretched the special-case "theories" so grimly taught and tested -- and **invocations about programming style almost sure to be outmoded before the student graduates**.



(yup, quite true even today)





## The trouble with programming languages ...



> _SYNTAX IS OFTEN UNNECESSARY_. **One can survive with much less syntax than is generally realized**. Much of programming syntax is concerned with suppression of parentheses or with emphasis of scope markers. There are alternatives that have been much underused.

> ... such devices as infixes and operator precedence ... have their place. But _their importance to computer science as a whole has been so exaggerated that it is beginning to corrupt the youth_.



(I wonder what he would have thought of _C++_ !)



> The clumsiness, for humans, of writing all the parentheses is evident; **the advantages of not having to learn all the conventions ... is often overlooked**.

> It remains to be seen whether a syntax with explicit delimiters is reactionary, or whether it is the wave of the future.



(a possible nod to Lisp ?)



> Much can be done to clarify the structure of expressions in such a "syntax-weak" language by using indentation and other layout devices that are outside the language proper.



(a possible precursory nod to Python ?)





## Moving forward ...



> What is a compiler for? The usual answers resemble "to translate from one language to another" or "to take a description of an algorithm and assemble it into a program, filling in many small details." For the future, a more ambitious view is required. Most compilers will be systems that "produce an algorithm, given a description of its effect."



(not sure what to make of this)



> No matter how a language is described, a computer must use a procedure to interpret it. One should remember that _in describing a language the main goal is to explain how to write programs in int and what such programs mean_. The main goal _isn't_ to describe the syntax.



(Sure, ok)



> Paradoxically, though modern programming ideas were developed because processes were hard to depict with classical mathematical notations, designers are turning back to an earlier form -- the equation -- in just the kind of situation that _needs_ program.



(aha! wait, what?)





## Stay tuned



So much for Part 1. I'll pick up the rest of the extracts next time.


