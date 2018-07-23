{:title "monthly prog slash math slash sci summary march 2016"
:layout :post
 :tags ["old-post"]}



(_Meta_: This month's post is late because I was on vacation)



March's list of curated _stuff_:



- A [grab bag](http://ethw.org/Main_Page) of engineering and technology history: lots of interesting stuff here, such as [this overview](http://ethw.org/Creating_Magnetic_Disk_Storage_at_IBM) of early disk storage



- [A talk](https://www.youtube.com/watch?v=RdE-d_EhzmA) by David West on how "Object Oriented Programming" has been misunderstood (I ended up buying his book [_"Object Thinking"_](http://www.amazon.com/Object-Thinking-Developer-Reference-David/dp/0735619654))



- Stumbled across this PhD thesis from 1991 by John Sturdy titled [_"A Lisp through the Looking Glass"_](http://www.cb1.com/~john/thesis/thesis.html); it seemed vaguely similar to the _Black_ system in Scheme that Nadia demoed a while ago at Strangeloop. Anyway, while I don't quite understand it, there is much coolness here.



> This thesis presents a new architecture for programming language interpreters, **in which interpreters are not only first-class values, but are also arranged in a tower of meta-circular interpretation** which is accessible reflectively---so that a program may modify elements of the meta-circular tower under which it runs, and thus cause changes in the manner of its own interpretation.

>

> To facilitate such modification, we develop a representation for interpreters that splits each interpreter into a language (a collection of independently implemented constructs) and an evaluator (connecting the constructs together).

> 

> To implement such a mutable infinite meta-circular interpreter, we need another interpreter outside the tower, the meta-evaluator. We present this, along with a systematic way of linking it to the meta-circular tower. We show that a further form of meta-circularity may be introduced by bringing the meta-evaluator into the reflectively accessible part of the system; and that this may be repeated without limit, using the same techniques.



- Gregory Chaitin wrote an essay on [_"Doing Mathematics Differently"_](http://inference-review.com/article/doing-mathematics-differently), in which he seems to encourage an experimental branch of mathematics, similar to physics.



- Another essay, this time by Simon Schaffer titled [_"Babbage's Intelligence"_](http://www.imaginaryfutures.net/2007/04/16/babbages-intelligence-by-simon-schaffer/), which is ridiculously long and meanders endlessly, and is worth reading only for the fascinating nuggets buried here and there.



> In his address to the Society in early 1824, the Society’s president, the financier, mathematician and orientalist Henry Colebrooke, summed up the significance of Babbage’s planned device. He referred to contemporary developments of industrial machinery only to contrast them with the Difference Engine. “In other cases, mechanical devices have substituted machines for simpler tools or for bodily labour….But the invention to which I am adverting comes in place of mental exertion: it substitutes mechanical performance for an intellectual process”. In other words, **“Mr Babbage’s invention puts an engine in place of the computer”**.



> As Babbage and his allies among the political economists showed, the disaggregation of the production process into its simplest components allowed a series of economies and practices of surveillance. Mechanized production required strict discipline. The same was true of the Analytical Engine. Parcelling the processes of Lagrangean algebra into specific components allowed the increase in speed of the machine, the transformation of infinities of space into manageable durations of time, the most economical recompense to each component in terms of consumed power (if mechanical) or consumed wages (if human). “**The whole history of the invention has been a struggle against time**”, Babbage wrote in 1837. The replacement of individual human intelligence by machine intelligence was as apparent in the workshop as in the engines. In the former, this task was both politically and economically necessary. “One great advantage which we derive from machinery is the check which it affords against the inattention, idleness or the dishonesty of human agents”. This set of failings could produce erroneous astronomical tables, hence the significance of Prony’s reports on the performance of the least intelligent computers when subject to the right management. Unreliable agents could also form trade union combinations, which, Babbage held, were always “injurious” to the workforce itself. His aim here was to contest the influence of “designing persons” and show the working classes that **“the prosperity and success of the master manufacturer is essential to the welfare of the workman”**, even though “I am compelled to admit that this connexion is in many cases too remote to be understood by the latter”.



> This problem of the geography of intelligence depended on the fetishisation of the machines and the reification of the labour power exerted around them. As Raphael Samuel has demonstrated, mid-Victorian industrial mechanization was accompanied by the preservation, intensification and expansion of skilled manual labour throughout the economy. “The mid-Victorian engineer was still characteristically a craftsman, an artisan or mechanic rather than an operative or hand”. [67] The representation of this dual process of the intensification of skill and the subordination of mechanization involved a remarkable balancing act amongst the commentators on the factory system. In the report of his Lancashire tour during the Chartist general strike of 1842, in which almost every cotton works was closed, Cooke Taylor premised that “the diffusion of the Factory System has created a larger demand than previously existed for intelligence and contrivance” among the workforce, and deduced that **the machines themselves could not, or should not, be granted tyrannic power**. “The operatives are stringently ruled by their own consent…So strange a combination of perfect despotism with perfect freedom never before existed, and to have produced such a state is one of the noblest triumphs of morality and intelligence”. [68] **The problem remained. Whose intelligence had produced this splendid state of voluntary servitude and supreme skill? Protagonists of the cotton masters had no doubt – it flowed from the machines themselves**. Thus Edward Baines, a veteran lecturer against the Chartists on the benefits of rapid automation, argued in his history of the cotton industry that “all the precision, power and incessant motion belong to the machines alone, and the work-people have merely to supply them with work”. The embodiment of skill within the automatic system was used to distract attention from the labour power exerted by the workforce itself. The most hostile critics of the machine economy, such as the Liberal Manchester medic Peter Gaskell, countered that it followed that any worker would be **“reduced to a mere watcher or feeder of his mighty assistant”, and that “the struggle carrying on between human power on the one hand, and steam aided by machinery, is gradually approaching a crisis”**.



> The apotheosis of the intelligent machine was an integral part of Babbage’s ambitious programme. This programme has been used here to illuminate the complex character of systematic vision in the Industrial Revolution. In the Ninth Bridgewater Treatise, **the system was coextensive with the universe, and Babbage explained that its order and logic would only be visible from a privileged point of view**. In his surveys of the factories and workshops, Babbage set out to reveal the systematic character of the machine economy by pointing out the rationale of the production, distribution and deployment of power in the workshops of industrial Britain. In his project to build intelligent calculating engines, he attempted to represent himself as the intellectual manager of the complex labour relations of the machine-tool industry, initially disastrously, and then as part of his overall vision of a newly rational system of automatic precision engineering. In the setting of early Victorian society, **the connections between these spheres of theological, political and technological work cannot be seen as merely metaphorical. These techniques helped make a new social order and a new form of knowledge.**



- A fascinating [account](http://www.gmunk.com/OBLIVION-GFX) of the special effects work on the movie _"Oblivion"_



- Dave Baggett at Gamasutra describes [_"my hardest bug ever"_](http://www.gamasutra.com/blogs/DaveBaggett/20131031/203788/My_Hardest_Bug_Ever.php), where setting the timer to a high enough clock rate interfered with the baud rate (??). I would hate to have a problem like that.



- Luke Gorrie shows how a modern CPU is complicated enough [to be thought of](https://github.com/lukego/blog/issues/15) as a distributed programming system.



Finally, if you too get irritated by the mention of the word _"agile"_, [Robert Martin explains](https://www.youtube.com/watch?v=hG4LH6P8Syk) how it started and how it became so bad.


