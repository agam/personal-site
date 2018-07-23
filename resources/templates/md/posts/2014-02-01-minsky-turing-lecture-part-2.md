{:title "minsky turing lecture part 2"
:layout :post
 :tags ["old-post"]}



(This picks up where [Part 1](http://agam.github.io/blog/2014/01/26/minsky-turing-lecture-part-1/) left off)



If you thought Minsky cared a lot about how programming languages work, he cares even more about how education "works". I recently picked up [The Society of the Mind](http://www.amazon.com/The-Society-Mind-Marvin-Minsky/dp/0671657135) so it seems a little less surprising, but read on ...



## Computer Science and Educational models



> He [the computer scientist] perceives his principal function to provide programs and machines for use in old and new educational schemes. Well and good, but I believe he has a more complex responsibility -- to work out and communicate models of the process of education itself.



And lest anyone doubt this ...



> We cannot digress here to answer skeptics who feel it too simpleminded (if not impious, or obscene) to compare minds with programs. We can refer many such critics to [Turing's paper](http://www.loebner.net/Prizef/TuringArticle.html). For those who feel that the answer cannot lie in any machine, digital or otherwise, one can argue that machines, when they become intelligent, very likely will feel the same way.



(If I might digress a bit, there is a very interesting footnote here by Minsky that might completely change the image in your head of [Turing](http://en.wikipedia.org/wiki/Alan_Turing): " **Turing was quite good at debugging hardware**. He would leave the power on, so as not to lose the 'feel' of the thing. Everyone does that today, but it is not the same thing now that the _circuits all work on three or five volts_.")



> There is a fundamental pragmatic point in favor of our propositions. The child needs models ...

...

> But to model his own self he _cannot_ use the engine or the organism or the city or the telephone switchboard; _nothing will serve at all but the computer with its programs and their bugs_



## Children and Mathematics



> Imagine a small child ... 

...

> If we extrapolate today's trend, his mathematical education will be conducted by poorly oriented teachers and, partly, _by poorly programmed machines_; neither will be able to respond to much beyond "correct" and "wrong" answers, let alone to make reasonable interpretations of what the child does or says, because neither will contain good models of the children, or good theories of children's intellectual development.



He goes on to give examples of how the child's geometric world is _so different_ from ours, such as some numbers having a different _weight_ or _depth_ than others, or not knowing how squares and diamonds are the same shape, as shown below:



{% img center http://farm6.staticflickr.com/5529/12246677434_860cbfa222_z_d.jpg %}



This builds up to something I really like (if I may let my personal biases show, I like it because it validates some of my own beliefs): the idea that computational analogies can (and indeed _should!_) be more generally used to describe patterns of thought and behavior.



> Before computation, the community of ideas about the nature of thought was too feeble to support an effective theory of learning and development. Neither the finite-state models of the Behaviorists, the hydraulic and economic analogies of the Freudians, nor the rabbit-in-the-hat insights of the Gestaltists supplied enough ingredients to understand so intricate a subject. It needs _a substrate of already debugged theories and solutions_ of related but simpler problems. Now we have a flood of such ideas, _well defined and implemented_, for **thinking about thinking**; only a fraction are represented in traditional psychology:



Some of the ideas listed are



* microprogram

* interrupt

* garbage collection

* macro

* symbol table



He goes on:



> All these serve today as tools of a curious and intricate craft, **programming**. But just as astronomy succeeded astrology, following Kepler's regularities, the discovery of _principles in empirical explorations of intellectual process in machines_ should lead to a science.



This finally brings him to a polemic against the nature of math education itself.



## Against the "New Mathematics" curriculum



What Minsky seems to be railing here is the excessive formalism in Mathematics introduced early on, where abstract concepts like sets are introduced before trying to create a _familiarity_ with numbers.



> Because of the formalistic approach the teacher will not be able to help the child very much with problems of formulation. For she will feel insecure herself as she drills him on such matters as the _difference between the empty set and nothing_, or the distinction between the "numeral" 3+5 and the numeral 8 which is the "common name" of the number eight, hoping he will not ask what is the common name of the fraction 8/1, which is probably different from the rational 8/1 and different from the quotient 8/1 ...



Here, the fractions, quotients and rationals obviously _should_ be identified with each other, rather than siloed and separated by this "maze of formalism". Also ...



> It is very bad to insist that the child keep his knowledge in a simple ordered hierarchy. In order to retrieve what he needs, he must have a multiply connected network, so that he can try several ways to do each thing ... Emphasis on the "formal proof" is destructive at this stage, because the **knowledge needed for finding proofs, and for understanding them, is far more complex (and less useful) than the knowledge mentioned _in_ proofs.**



Minsky suggests that this might stem from a misconception of the form "You can teach a child anything, if you just get the definitions precise enough".



He doesn't care too much about overly focusing on set theory either:



> Set theory is not, as the logicians and publishers would have it, _the_ only and true foundation of mathematics; it is a viewpoint that is pretty good for investigating the transfinite, _but undistinguished for comprehending the real numbers, and quite substandard for learning about arithmetic, algebra, and geometry_.



If there is a way to sum up his argument with one line, it is probably this:



> The child is expected to learn how to solve problems but we do not teach him what we know, either about the subject, or about problem-solving.



## Contradiction and Solution



> There is a real conflict between the logician's goal and the educator's. The logical wants to minimize the variety of ideas, and doesn't mind a long, thin path. The educator (rightly) wants to make the paths short and doesn't mind -- in fact, prefers -- connection to many other ideas. _And he cares almost not at all about the directions of the links._



So where does the computer scientist come in to this ? In a heroic way !



> The computer scientist thus has a responsibility to education. Not as he thinks, _because he will have to program the teaching machines_. Certainly not because he is a skilled user of "finite mathematics". _He knows how to debug programs_; he must tell the educators how to help the children to debug their own problem-solving processes. _He knows how procedures depend on their data structures_; he can tell educators how to prepare children for new ideas.
