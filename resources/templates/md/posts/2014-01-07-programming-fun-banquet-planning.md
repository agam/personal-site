{:title "programming fun banquet planning"
:layout :post
 :tags ["old-post"]}



Here's [another](http://www.reddit.com/r/dailyprogrammer/comments/1rnrs2/112813_challenge_137_intermediate_hard_banquet/) daily programming problem:



```

Input Description

-----------------



On standard console input, you will be given two space-delimited integers, N and M. N is the number of food items, while M is the number of food-relationships. Food-items are unique single-word lower-case names with optional underscores (the '_' character), while food-relationships are two food items that are space delimited. All food-items will be listed first on their own lines, then all food-relationships will be listed on their own lines afterwards. A food-relationship is where the first item must be served before the second item.

Note that in the food-relationships list, some food-item names can use the wildcard-character '*'. You must support this by expanding the rule to fulfill any combination of strings that fit the wildcard. For example, using the items from Sample Input 2, the rule "turkey* *_pie" expands to the following four rules:

turkey almond_pie

turkey_stuffing almond_pie

turkey pecan_pie

turkey_stuffing pecan_pie

A helpful way to think about the wildcard expansion is to use the phrase "any item A must be before any item B". An example would be the food-relationship "*pie coffee", which can be read as "any pie must be before coffee".

Some orderings may be ambiguous: you might have two desserts before coffee, but the ordering of desserts may not be explicit. In such a case, group the items together.



Output Description

------------------



Print the correct order of food-items with a preceding index, starting from 1. If there are ambiguous ordering for items, list them together on the same line as a comma-delimited array of food-items. Any items that do not have a relationship must be printed with a warning or error message.



```



The second sample input (to take an example) is as follows:



```

8 5

turkey

pecan_pie

salad

crab_cakes

almond_pie

rice

coffee

turkey_stuffing

turkey_stuffing turkey

turkey* *_pie

*pie coffee

salad turkey*

crab_cakes salad

```



So this implies a graph that looks something like this:



{% img center /images/programming-fun/graph-dependency-1.jpg 300 %}



Or, if you wish, with the arrows reversed, like this:



{% img center /images/programming-fun/graph-dependency-2.jpg 300 %}



So it's possible to start from the first `root` node, labelling it as `0` and its neighbors as `1`. This process can be repeated, with its neighbors' neighbors being labelled as `2`, and so on, until there are no nodes left to label.



{% img center /images/programming-fun/graph-dependency-3.jpg 300  %}



Once all nodes are labelled, the solution is just a pairing of these numbers and the corresponding nodes.



{% gist 8293739 %}


