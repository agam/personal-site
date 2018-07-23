{:title "small change for vim and git"
:layout :post
 :tags ["old-post"]}



Vim

+++



I hate seeing `.swp` and `~` files when I'm editing. If I really need to go back to an earlier version of something there are other ways.



So I added this to my .vimrc:



```

set nobackup

set nowritebackup

set noswapfile

```



Git

+++



If there's one config change to make for `git`, it should be this one:



```

$ git config --global --add color.ui true

```


