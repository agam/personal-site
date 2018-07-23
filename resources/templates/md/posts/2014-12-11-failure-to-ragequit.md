{:title "failure to ragequit"
:layout :post
 :tags ["old-post"]}





Migrating from my Github blog (in Octopress) was supposed to work pretty much as specified in [this guide](http://www.stackednotion.com/blog/2014/02/26/migrating-from-octopress-to-wordpress/) [^1]



- I used [this gist](https://gist.github.com/lucaspiller/9229838#file-rss-xml) in `source/rss.xml` and then ran `rake generate`

- I imported the generated file (in `public/rss.xml`) into my Wordpress blog, only to see this error

> This does not appear to be a WXR file, missing/invalid WXR version number.

- A helpful post on [the Wordpress Forums](https://wordpress.org/support/topic/xml-to-wxr-issue) suggested the following change



```

<rss xmlns:dc="http://purl.org/dc/elements/1.1/" version="2.0">

  <channel>

    <atom:link rel="hub" href="http://pubsubhubbub.appspot.com" xmlns:atom="http://www.w3.org/2005/Atom"/>

    <wp:wxr_version>1.1</wp:wxr_version>

    <title>Agam&#39;s Mashed-Up Pome</title>

```



(i.e. adding the `wxr_version` marker line after the language specification)



- After this, I tried importing again, it gave some warnings about not recognizing the author (which was to be expected) ... and then though it said "import successful!", there were no posts to be seen!



**WTF Aaaarrgh WTF**



date = "An"

slug = "An/failure-to-ragequit"



Yeah, I don't have the _$&@%in_ time to waste trying various other hacks. The big reasons I wanted to switch was that



1. I don't want to muck around with Ruby _ever again_, and

2. I want to be able to write posts _online_ (i.e. through some sort of browser interface).



Now (1) is not an immediate problem, so I suppose if it re-occurs, I'll put more time/effort into quitting Octopress, but meanwhile there are compromises possible for (2), such as the nice interface of [draftin.com](http://draftin.com) that I _just_ discovered. _So yeah, this blog lives on ..._



[^1]: This is the _second_ time I've tried to jump from Octopress to Wordpress; Google suggests that most people seem to be [going the opposite direction](https://www.google.com/search?q=migrate+%22from+wordpress+to+octopress%22&oq=migrate+%22from+wordpress+to+octopress)
