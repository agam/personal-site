{:title "adding social elements to octopress"
:layout :post
 :tags ["old-post"]}



This is almost too easy. Octopress already comes with basic integration options built-in.



See [here](http://jekyllrb.com/docs/configuration/) for more details than I care about, but the short version is that you just have to edit `_config.yml` and fill in fields for your Google+ username, a true/false value indicating whether users can share your posts on Google+/Twitter, a Disqus shortname for comments, a Google Analytics tracking code ... you get the idea.



(though there seems to be some [extra work](http://blog.jmac.org/blog/2013/03/30/putting-twitter-back-into-octopress/) required to get Twitter to show up in the sidebar.



While we're at it, might as well change the theme too. There's a huge collection of options at [OPThemes](http://opthemes.com/). So, pick one and replace the default dark grey one!



The usual flow is something like `git submodule add <the theme's git repo>` followed by `rake install["<the theme name>"]`



I picked [`octoflat`](http://alexgaribay.com/) which doesn't have a sidebar, but is otherwise pleasing to look at.



Happy customizing!


