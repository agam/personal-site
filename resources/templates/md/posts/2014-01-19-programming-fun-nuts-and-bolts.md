{:title "programming fun nuts and bolts"
:layout :post
 :tags ["old-post"]}



[This one](http://www.reddit.com/r/dailyprogrammer/comments/1sob1e/121113_challenge_144_easy_nuts_bolts/) is _too easy_ but included anyway for completeness (why not?)



```

Input Description

-----------------



The first line of input will be an integer N, which is for the number of rows

in each list. Each list has N-lines of two space-delimited strings: the first

string will be the unique item name (without spaces), the second string will be

the price (in whole-integer cents). The second list, following the same format,

will have the same unique item-names, but with the correct price. Note that the

lists may not be in the same order!



Output Description

------------------



For each item that has had its price changed, print a row with the item name

and the price difference (in cents). Print the sign of the change (e.g. '+' for

a growth in price, or '-' for a loss in price). Order does not matter for

output.



```



{% gist 8498879 %}


