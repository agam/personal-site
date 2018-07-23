{:title "problems with octopress in a new location"
:layout :post
 :tags ["old-post"]}



My git-n00bness obviously inhibited me a bit here, but I was today in the situation of creating a new location from which to blog (a hosted VM, more about which in a separate post).



So I ran `git clone https://github.com/agam/agam.github.io.git`, which gave me the `master` branch.



But I also needed the `source` branch. So I naively ran `git pull origin source`, which resulted in ... a merge conflict on `index.html`?! WTF?



I 'fixed' that by making `index.html` empty. But `rake generate` failed with weird errors and `git branch` still shows only `source`. So what was wrong?



[This stackoverflow post](http://stackoverflow.com/questions/67699/how-do-i-clone-all-remote-branches-with-git) had a possible answer on how to proceed.



I tried running `git checkout origin/source` but that brought on the dreaded `experimental HEAD` error message.



No luck. Then I came across [this post](http://blog.zerosharp.com/clone-your-octopress-to-blog-from-two-places/) which seemed to describe a similar situation (I crossed my fingers that I hadn't already screwed up my repo).



Aha! (I won't forget it now). You're supposed to specify the branch name when you check out!



So I `rm -rf`ed my setup, then ran the following:



```sh

git clone -b source https://github.com/agam/agam.github.io.git

cd agam.github.io

git clone https://github.com/agam/agam.github.io.git _deploy

```



After this, `rake new_post` worked as expected, but `rake generate` still failed on some plugin error about an "invalid byte sequence".



That led me to [this gist](https://gist.github.com/arnab/4584901). `export LANG=en_US.utf-8` fixed the generate problem.



And then `rake deploy` worked as expected, so yay!


