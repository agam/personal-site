{:title "programming fun graph distances"
:layout :post
 :tags ["old-post"]}



Interesting [problem](http://www.reddit.com/r/dailyprogrammer/comments/1tiz4z/122313_challenge_140_intermediate_graph_radius/): given a graph, find out the _minimum distance between any two nodes_.



```

Input Description

-----------------

On standard console input you will be given an integer N, followed by an

Adjacency matrix. The graph is not directed, so the matrix will always be

reflected about the main diagonal.



Output Description

------------------

Print the radius of the graph as an integer.

```



There are a few ways to do this -- you can run [Dijkstra's algorithm](http://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) or [Floyd-Warshall algorithm](http://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm) on the graph to find the path from each node to each other node. I sort of took a short cut -- when the graph is represented as a matrix (as it is here), multiplying the matrix with itself (and adding the result to itself, so as not to lose the _0-step_ info) can be used as a way of checking whether or not two nodes are connected.



Concretely, after `k` steps, if `Graph[i][j]` is `0` this means there is no path from `i` to `j` in `k` steps. So if you repeat this multiplication and stop when there are no zero elements, the number of multiplication steps is the minimum distance you need.



{% gist 8794234 %}



[I'm a little bored of these daily programmer questions now :( I'll do one more hard one and then find a new theme here.]


