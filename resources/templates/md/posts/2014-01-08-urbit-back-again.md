{:title "urbit back again"
:layout :post
 :tags ["old-post"]}



Turns out I was affected by the last "continuity breach".



So I had to delete everything and start over. In my case this meant



```sh

$ cd urbit

$ git pull origin master

$ make clean

$ make

$ rm -rf urb/*-*

$ rm -rf frogstar

$ vere -c frogstar

```



This time when I ran `:begin` it succeeded



```sh

generating 2048-bit RSA key...

; ~doznec _Urban Republic_ is your neighbor

; ~doznec _Urban Republic_ is your neighbor

request approved

; ~zod |Tianming| is your neighbor

; ~zod |Tianming| is your neighbor

; ~doznec _Urban Republic_ is your neighbor

; ~doznec _Urban Republic_ is your neighbor

```



... and then more hoon files were downloaded, after which I got my new prompt:



```sh

~todbyt-nathes/try=> 

```



Next time I'll start exploring *Nock* and *Hoon* some more ...
