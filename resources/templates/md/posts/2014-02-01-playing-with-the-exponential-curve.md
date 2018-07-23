{:title "playing with the exponential curve"
:layout :post
 :tags ["old-post"]}



I remember hearing about the [apocryphal story](http://britton.disted.camosun.bc.ca/jbchessgrain.htm) about doubling grains of rice on a chessboard. I even tried writing powers of 2 (which at my age then just meant adding `2+2` then `4+4` etc).



{% img center http://britton.disted.camosun.bc.ca/firdau1.jpg %}



This is obviously the [`e^x`](http://www.wolframalpha.com/input/?i=e+%5E+x) curve, but when you normally see it drawn its self-similarity is not obvious. Mathematica's `Manipulate` function can help in this regard ...



```

Manipulate[ListPlot[Table[Power[2, i], {i, 1, n}]], {n, 1, 64}]

```



{% img center http://farm3.staticflickr.com/2853/12244178256_251f9fcfae_d.jpg %}

{% img center http://farm6.staticflickr.com/5496/12243600435_e6faac3bbb_d.jpg %}

{% img center http://farm6.staticflickr.com/5547/12244178226_2258c2c2ce_d.jpg %}

{% img center http://farm3.staticflickr.com/2882/12243785163_3822d7299b_d.jpg %}

{% img center http://farm8.staticflickr.com/7332/12243785183_6243378edf_d.jpg %}



These are screenshots for increasing values of `n`: Note how they all look the same (except for the first one, where the curve is just starting out), over several orders of magnitude!


