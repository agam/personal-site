{:title "euler 29 distinct powers"
:layout :post
 :tags ["old-post"]}



**Statutory Warning**: _Spoilers ahead!_



Finally, we have a one-liner:



```haskell

euler29 = length $ L.nub $ [a ^ b | a <- [2 .. 100], b <- [2 .. 100]]

```



(where `L` is `Data.List`)



_P.S._ I did make a dumb error by initiall writing this as two functions, with the second one taking `Int` instead of `Integer`, which (I'm disappointed to say) resulted in everything silently overflowing to negative numbers.
