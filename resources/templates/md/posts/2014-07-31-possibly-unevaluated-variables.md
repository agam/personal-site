{:title "possibly unevaluated variables"
:layout :post
 :tags ["old-post"]}



```scala

scala> def loop: Boolean = loop

<console>:10: warning: method loop does nothing other than call itself recursively

       def loop: Boolean = loop

                           ^

loop: Boolean



scala> true || loop

res34: Boolean = true



scala> true && loop



[need to Ctrl-C out at this point]

```



(I like that warning message!)
