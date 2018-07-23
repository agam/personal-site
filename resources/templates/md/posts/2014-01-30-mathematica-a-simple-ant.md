{:title "mathematica a simple ant"
:layout :post
 :tags ["old-post"]}



Here's a simple, exploratory sketch: Imagine an ant which can either move left or right, or stay in its place.



For simplicity, we'll imagine the ant starts at the center of a scale from `1` to `100`, and moves one unit at a time.



```

Ant[k_, u_] := 

 If[ u < 1/3, Min[k + 1, 100], If[u > 2/3, Max[0, k - 1], k]]



RecAnt[k_] := Ant[k, RandomReal[]]

```

 

 {% img center http://farm8.staticflickr.com/7351/12224251723_4d24311967_d.jpg %}

 

 ```

 ListPlot[NestList[RecAnt, 50, 100000], ImageSize -> Large, Background -> LightGray]

 ```

 

 {% img center http://farm8.staticflickr.com/7418/12224482144_6ef516fd03_z_d.jpg %}
