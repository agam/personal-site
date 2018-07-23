{:title "golang setup"
:layout :post
 :tags ["old-post"]}



Just some basic personal notes here.



Obviously, get the `go-mode` for Emacs. It has useful shortcuts, like `C-c C-a` to import a package while you're editing some code and don't want to scroll up and add this, or `C-c C-d` to show the signature of the function at point. There are other useful functions (for which you can add your own shortcuts), such as `gofmt` which does what you'd expect, and `go-play-region`, which is nifty (but perhaps not that useful): it sends the selected code to [the Go Playground](http://play.golang.org/), and displays the result.



It'll also be useful to get `godef`. So, just `go get code.google.com/p/rog-go/exp/cmd/godef` (remember to set `$GOPATH` first -- I use `~/Golang/Packages`).



If you're enthusiastic about further Emacs-Go integration, you can try out [`goflymake`](http://github.com/dougm/goflymake) -- I haven't used it myself (the compile step is nearly-instantaneous for me anyway), but I've heard its good, so YMMV.






