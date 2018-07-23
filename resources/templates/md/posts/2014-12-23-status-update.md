{:title "status update"
:layout :post
 :tags ["old-post"]}



Since the FreeBSD experiment didn't work out[^1], I picked a different one: "plain old" Debian[^2]! (should be _acronymized_ to POD).



I read an old piece of evangelism[^3] which gave me the idea to version-control `/etc`, though I have sinced found `etckeeper` to do the same[^7]



```sh

$ sudo apt-get install etckeeper

 

# Modify VCS to `git` (not necessary on Debian, AFAICS)

$ sudo vi /etc/etckeeper/etckeeper.conf

 

$ cd /etc

$ sudo git log

# Should show "...blah blah blah ... Initial Commit"

# On Ubuntu, might need to explicitly run `sudo git commit -a -m 'Initial commit`

 

# Fully integrated with APT

$ sudo git log

# will show something like "... committing changes in /etc after apt run ..."

```



Other than that, not much else. Busy with a new baby daughter (!)[^4], trying to read less "news and blogs"[^5], pretending to read a book on NLP[^6], and having a "brand new linux box" experience with a VM in Google Cloud.



Have a happy new year, everyone.



[^1]: [http://agam.github.io/blog/2014/12/14/trying-out-freebsd-and-failing-at-it/](http://agam.github.io/blog/2014/12/14/trying-out-freebsd-and-failing-at-it/)

[^2]: [https://www.debian.org/](https://www.debian.org/)

[^3]: [https://wiki.debian.org/WhyDebian](https://wiki.debian.org/WhyDebian)

[^7]: [https://gist.github.com/agam/2cda8597966f49a04e07](https://gist.github.com/agam/2cda8597966f49a04e07)

[^4]: [https://plus.google.com/108609219349604504501/posts/1CVG1iFTtXe](https://plus.google.com/108609219349604504501/posts/1CVG1iFTtXe)

[^5]: [http://www.theguardian.com/media/2013/apr/12/news-is-bad-rolf-dobelli](http://www.theguardian.com/media/2013/apr/12/news-is-bad-rolf-dobelli)

[^6]: [http://nlp.stanford.edu/fsnlp/](http://nlp.stanford.edu/fsnlp/)
