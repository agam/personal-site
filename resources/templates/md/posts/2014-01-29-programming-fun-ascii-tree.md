{:title "programming fun ascii tree"
:layout :post
 :tags ["old-post"]}



Another [easy problem](http://www.reddit.com/r/dailyprogrammer/comments/1t0r09/121613_challenge_145_easy_tree_generation/) (I'm not picking easy problems! I'm just going in sequence!)



```

Input Description

-----------------

You will be given one line of text on standard-console input: an integer and

two characters, all space-delimited. The integer, N, will range inclusively

from 3 to 21 and always be odd. The next character will be your trunk

character. The next character will be your leaves character. Draw the trunk and

leaves components with these characters, respectively.



Output Description

------------------

Given the three input arguments, draw a centered-tree. It should follow this

pattern: (this is the smallest tree possible, with a base of 3)

   *

  ***

  ###

Here's a much larger tree, of base 7:

   *

  ***

 *****

*******

  ###

```



Is this really programming at all? More like basic _arithmetic_. Anyway ...



{% gist 8679031 %}



[Also, I realized I'm wasting everyone's time by having all those redundant `#include` lines, so I got rid of them here]
