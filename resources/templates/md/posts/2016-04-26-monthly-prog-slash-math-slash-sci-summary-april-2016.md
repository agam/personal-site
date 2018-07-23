{:title "monthly prog slash math slash sci summary april 2016"
:layout :post
 :tags ["old-post"]}



Ok, so here’s this month’s curated list of interesting Math/Sci/Programming _stuff_. **Note**: this isn’t stuff that _happened_ this month, it’s stuff that _I came across_ this month.



- To start off with something _fun_, Gilad Bracha (I try not to miss anything by him) [joins the discussion](https://devchat.tv/ruby-rogues/250-rr-programming-language-evolution-and-design-with-gilad-bracha) at _Ruby Rogues_, they all talk about Dart and Smalltalk and Newspeak, and much hilarity ensues. So, if you’re into that sort of thing.



> GILAD:  And the thing is once you get used to the machine doing it for you it's really hard to go back to the Stone Age. Smalltalk or Smalltalk-like systems, they give you in certain ways the life of luxury. It's so nice that you can, when something goes wrong you can always find out exactly what the state of the objects were and say, “Oh, this is wrong.” And I don't have to restart things to get back there if I figured out what's wrong or I have a hunch what's wrong. I can just change it and tweak it and go forward a lot of times, which is something that again you have to experience because people tend to not believe that. But you usually can not only make the change but you can very often keep going even though the program was wrong. You can adjust its state now a bit and say, “Okay, let's assume that this is now consistent. I can keep moving and see what happens.” **And you get so comfortable with that and it's so much more pleasant that after that yeah, you ramble on about it like I do.**



Yeah, watch it, it's all good stuff.



- Something to jog your preconceived notions about what “object oriented” means: a paper from twenty years ago, titled [_”Classes vs. Prototypes - Some Philosophical and Historical Observations”_](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.56.4713)



> The distinction between class-based and prototype-based systems reflects a long-lasting philosophical dispute concerning the representation of abstractions. **Plato viewed forms — stable, abstract, “ideal” descriptions of things — as having an existence more real than instances of those things in the real world**. Class-based languages such as Smalltalk, C++ or Simula are Platonic in their explicit use of classes to represent similarity among collections of objects. Prototype-based systems such as Self [UnS87], Omega [Bla91, Bla94], Kevo [Tai92, Tai93], GlyphicScript [Gly94] and NewtonScript [SLS94] represent **another view of the world, in which one does not rely so much on advance categorization and classification, but rather tries to make the concepts in the problem domain as tangible and intuitive as possible**.





- And if you liked that, you’ll like [this discussion](https://www.youtube.com/watch?v=8nfrC-YLYqc) with David Ungar, the man behind the _Self_ programming language. In case you never heard of it before (as I hadn’t, until recently), that’s where Javascript gets its concept of _”prototype”_ from. And if you’re _still_ hungry for more, see [this presentation](http://www.infoq.com/presentations/oop-language-context) titled _”Dancing with Symmetry to Harness the Power of Complexity: Subjective Programming in Context”_



- A set of ten [“learnings”](http://www.allthingsdistributed.com/2016/03/10-lessons-from-10-years-of-aws.html) from ten years of AWS. If I had to pick, I’d say the hardest directive would be to _”build evolvable systems”_, because that almost never happens.



- Someone finally wrote what I've been thinking for a while ... the answer for "how to do X" can't always be "oh, just use plugin Y” (or, [_”How to overcome intuition in programming”_](http://amasad.me/2016/01/03/overcoming-intuition-in-programming/)).



- Marvin Minsky passed away early this year; [here](http://web.media.mit.edu/~minsky/papers/TuringLecture/TuringLecture.html) is his 1970 Turing Award lecture, which opens with this interesting sentence:



> The trouble with computer science today is an obsessive concern with form instead of content.



It’s as true today as it was then, right? :)



- This is something I’ve come across many times before, so skip it if you’ve seen it, or skip it anyway; it’s basically a bunch of Haskell insider jokes, laid out as [_”The Evolution of a Haskell Programmer”_](http://www.willamette.edu/~fruehr/haskell/evolution.html), starting from a freshman, going all the way to a post-doc, and ending, on an anti-climactic note, with the tenured professor.



- This one fulfills this month’s “rant quota”: Brian Will, slightly infamous for ranting against Object-Oriented Programming, talks about [_”Replacing the Unix tradition”_](https://www.youtube.com/watch?v=L9v4Mg8wi4U&list=WL&index=6)



- Another post in the _counter-intuitive_ category: [the best defense](http://ib-krajewski.blogspot.com/2015/11/ocaml-and-multithreading.html) I’ve yet seen for _not_ using shared-memory multithreading, using OCaml as an example.



> … parallel processing plus message passing is a much safer model that multithreading …



- Finally, to end on a similar note to which we began, the amazing Eugenia Cheng (look her books up) talks about [_”How to Bake 'How to Bake Pi': Reflections on Making Abstract Math Palatable”_](https://www.youtube.com/watch?v=h2p68JjSUH0); it’s targeted at people of all ages, and tries to make math fun again!


