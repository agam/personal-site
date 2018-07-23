{:title "command line gists"
:layout :post
 :tags ["old-post"]}



Every once in a while I need to share some code snippets, and [Gist](https://gist.github.com/) has been the best way to do this so far. Using the browser wastes a few mouseclicks, especially if you already have the intended contents (either entirely or partially) in a text file, so here's a better way to do this:



```sh

sudo gem install gist



gist --login



gist myfile

```



A new gist containing the contents of `myfile` (along with the _appropriate source formatting_) will be created, and you should see something like





```sh

https://gist.github.com/3d82745c84c8f44dd51c   

```



indicating success.



**Note:** If you want to upload anonymously, try `gist -a myfile` instead of the above.
