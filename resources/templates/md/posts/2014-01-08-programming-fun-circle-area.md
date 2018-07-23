{:title "programming fun circle area"
:layout :post
 :tags ["old-post"]}



Straightforward *math-only, no-algorithm* [problem](http://www.reddit.com/r/dailyprogrammer/comments/1s6484/120513_challenge_138_intermediate_overlapping/)



```

Input Description

-----------------



On standard input you will be given four floating-point space-delimited values:

x y u w. x and y are the first circle's position in Cartesian coordinates. The

second pair u and w are the second circle's position.



Note that the given circles may not actually intersect. If this is the case,

return the sum of both circles (which will always be Pi x 2 since our circles

are unit-circles).



Output Description

------------------



Print the summed volume of the two circles, up to an accuracy of 4 digits after the decimal place.

```



This is just trigonometry, so I made some Post-It illustrations. This one shows the general problem we have to solve.



{% img center /images/programming-fun/circle-area-1.jpg 300 %}



Given the centres of the two circles, and their radii, we know *d*, the distance between their centres and whether it leads to overlap at all.



{% img center /images/programming-fun/circle-area-2.jpg 300 %}



Overlap only occurs when *d < 2r*. When *d > 2r* the answer is just the sum of the areas.



{% img center /images/programming-fun/circle-area-3.jpg 300 %}



From this figure, it should be clear that the overlapping area is the segment of the circle subtended by the chord that is common to both circles. We can find the length of this chord by considering the right triangle formed by half of it, the radius, and half the centre distance.



This then leads to the relationship between the area of the sector (*A*), the area of the triangle inside it (*A_1*) and the segment area (*A_2*)



{% img center /images/programming-fun/circle-area-4.jpg 300 %}



Finally, we subtract the segment area from the area of the circle (and then double it, for two circles) to get our answer.



{% img center /images/programming-fun/circle-area-5.jpg 300 %}





{% gist 8323896 %}


