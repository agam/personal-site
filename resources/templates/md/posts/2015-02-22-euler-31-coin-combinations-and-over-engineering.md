{:title "euler 31 coin combinations and over engineering"
:layout :post
 :tags ["old-post"]}



I had this one all wrong, and made it way more complex than it really was. I'm embarassed to post the final code ... but it is what it is.



**Statutory Warning**: _Spoilers ahead_



```haskell

import Data.Maybe



-- Basic idea is something like this:

-- [*] given amount and some coins, pick the largest coin

-- [*] there are (floor (total/coin)) ways of using this coin value

-- [*] but ... NOT ALL of these count! Only the ones that leave a total that can be used with the remaining coins!



coins = [1, 2, 5, 10, 20, 50, 100, 200]



sortedCoins :: [Int]

sortedCoins = L.reverse $ L.sort $ coins



combinations :: Int -> [Int] -> Maybe Int

combinations total coins =

--  T.trace ("total = " ++ show total ++ ", coins = " ++ show coins) $

  if total == 0

  then Just 1

  else case coins of

        [] -> Nothing

        [c] -> if mod total c == 0 then Just 1 else Nothing

        c:cs -> if total == 0

                then Just 1

                else let newTotal total coin num = total - (coin * num)

                         combs = [combinations (newTotal total c num) cs | num <- [0 .. div total c]]

                         counts = sum $ catMaybes combs

                     in

                      if counts == 0 then Nothing else Just counts



euler31 :: Int

euler31 = fromJust $ combinations 200 sortedCoins

```



It turns out, I was being overly cautious and could have just used lists in a different way (left in a commented out debug line to show that I needed it).
