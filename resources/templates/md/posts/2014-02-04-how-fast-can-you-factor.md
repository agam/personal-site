{:title "how fast can you factor"
:layout :post
 :tags ["old-post"]}



For no other reason than _because it's there_, here is the result of asking Mathematica to factor [RSA 100](http://en.wikipedia.org/wiki/RSA_numbers#RSA-100).



In 1991,



> ... the factorization took a few days using the multiple-polynomial quadratic sieve algorithm on a MasPar parallel computer.



and now,



> It takes four hours to repeat this factorization using the program Msieve on a 2200 MHz Athlon 64 processor.



In comparison, Mathematica took little more than an hour. Unfortunately, it was not able to parallelize it (for a _real_ comparison, the [Msieve code](http://sourceforge.net/projects/msieve/files/msieve/Msieve%20v1.51/) would have to be ported over).



{% img center http://farm6.staticflickr.com/5510/12309071334_575cda01f9_z_d.jpg  %}



**Update:** This has been much better documented on [this StackExchange thread](http://mathematica.stackexchange.com/questions/14964/time-approximation-of-decrypting-rsa-algorithm), which also links to [notes on internal implementation](http://reference.wolfram.com/mathematica/tutorial/SomeNotesOnInternalImplementation.html#12915), in case you're interested:



> FactorInteger switches between trial division, Pollard , Pollard rho, elliptic curve, and quadratic sieve algorithms.



Finally, more on RSA Numbers, as well as a Mathematica Notebook, [here](http://mathworld.wolfram.com/RSANumber.html)


