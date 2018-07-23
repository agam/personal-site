{:title "getting into programming"
:layout :post
 :tags ["old-post"]}



I saw an ad for Codecademy[^1] in the footer of some web page, and immediately had mixed reactions. I'm all for making programming a basic skill alongside reading and writing, but the focus of these "bootcamps"[^2] seems to be _vocational web development_[^3].



This is ~~fine~~great for people who really want to do that and until now lacked a way to acquire these skills in a focussed manner. But I'm worried that this will replace what _computer science_ or _programming_ mean to a whole generation of kids[^4].



(Note: to be fair, there are bootcamps which are exceptions to the rule[^5], that focus on a wider array of skills)



While tweaking web pages might offer an instant gratification that can be sufficient for most people, I hope there are lots of kids who'd like a bit more "hard core" approach. In the past, I have always believed this path to best consist of _C_ followed by _Python_ followed by _Scheme_.



This is still a great way to proceed, but I now think I have a better prescription: replace _Scheme_ with one of _Racket_[^6]/_Common Lisp_/_Clojure_/_Haskell_[^7], and replace _C_ and _Python_ by a single language: _Go_.



The more I think about it, the more I am convinced that Go is an _excellent_ way to learn programming. It fulfills the requirement of "learning about the machine" (since it's syntactically similar to C, but _safer_) and being quick to develop and iterate (as in Python, it's very easy to install external libraries, build, run, fix, run again, etc)



I'm thinking here of a 10-year old kid (ok, a 10-year old kid _who wants to learn_). There's stuff like Scratch[^8] and Logo[^9] for younger kids, and various programming tutorials aimed at slightly older kids, but what's the best way to target this kid? **I think it's by getting him/her to play around with (1) some flavor of unix, and (2) learn Go. Both are free.[^10]**



There's never been a better time to get acquainted with the nuts and bolts of computers -- you don't even need to get a real one! These days major "cloud providers"[^11] have some "free usage" tier, and an easy way to register for a single VM[^12].



The best part about using a VM instead of a "real computer" is that you don't have to worry about crashing it, or otherwise doing anything irreversible. Reached a dead end? No problem, delete the VM and re-provision it.



Which brings me to the second part, using Go. The Go playground[^14] makes an initial introduction dead simple, but getting up and running locally is just as straightforward. The syntax is very simple and it's easy to bang out lots of code and immediately see it working. (also, one of things a kid _won't_ have to learn with Go? _makefiles_!)



A question that remains is "what exactly to do with Go?", to which my answer is _"simple text-based games"_.



So yeah ... this is the train of thought I've been carrying about for the past few days. BTW ... it's New Year's Eve, so **happy new year!**[^15]



[^1]: [http://www.codecademy.com/](http://www.codecademy.com/)

[^2]: See this [exhaustive list](http://www.skilledup.com/articles/the-ultimate-guide-to-coding-bootcamps-the-exhaustive-list/) for a "30,000 feet" overview)

[^3]: E.g. "Deep Dive Coders" features _HTML5, CSS3, Github, javaScript, jQuery, Apache, MySQL, php_, while "Makers Academy" features _Ruby on Rails, HTML5, CSS3, Javascript, jQuery, Git, Agile and Lean_ (whatever "lean" means)

[^4]: By which I mean ages 9 to 90 (!!)

[^5]: E.g. "Fullstack Academy" -- though I'm not sure if the name necessarily means what it does, since "Refactor U" promises _Full-stack JavaScript, Node.JS, HTML/CSS_

[^6]: Which I think of as "Scheme + useful libraries"

[^7]: A contentious choice indeed; _more on this later(!)_

[^8]: [http://scratch.mit.edu/](http://scratch.mit.edu/)

[^9]: [http://el.media.mit.edu/logo-foundation/logo/turtle.html](http://el.media.mit.edu/logo-foundation/logo/turtle.html)

[^10]: as in speech, and as in beer

[^11]: hate that term, but I don't know what else to use

[^12]: and even the smallest droplet on DigitalOcean, which is more than enough for this purpose, has a monthly price that's about the cost of a Venti Frappucino

[^13]: Or if you really, _really_ want a physical computer there are some Linux distributions [tailored for kids](http://opensource.com/education/14/1/teaching-kids-linux), though my intention was explicitly to force a "command-line only" interface :)

[^14]: [https://play.golang.org/](https://play.golang.org/)

[^15]: Although -- thoughts like these are similar to _New Year's resolutions_, which means you need to consider the possibility that this entire post was conceived in a spirit of naive optimism.¯
