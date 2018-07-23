{:title "euler 28 spiral diagonals"
:layout :post
 :tags ["old-post"]}



This problem afforded a chance to be lazy and have fun.



There definitely is a pattern in the diagonals here: for a `3 x 3` matrix, the elements are 3, 5, 7, 9. For a `5 x 5` matrix, the additional elements are 13, 17, 21, 25.



You can see these as being (1 + 2), (1 + 4), (1 + 6), (1 + 8), and then (1 + 2 + 2 + 8), (1 + 4 + 4 + 8), (1 + 6 + 6 + 8), (1 + 8 + 8 + 8).



Similarly, for a `7 x 7` matrix, the numbers are 31, 37, 43, 49, which are (1 + 2 + 2 + 2 + 8 + 8 + 8), (1 + 4 + 4 + 4 + 8 + 8 + 8), (1 + 6 + 6 + 6 + 8 + 8 + 8), (1 + 8 + 8 + 8 + 8 + 8 + 8)



On the other hand, the problem mentions a `1001 x 1001` matrix, which is _tiny_, so why bother with these patterns? Just create the matrix and sum up the diagonals directly! (It's _just_ a million elements ... now if it was a few orders of magnitude higher, it would be a different story)



**Statutory Warning**: Spoilers ahead!



```haskell

import qualified Data.List as L

import qualified Data.Vector as V

import qualified Data.Matrix as M

import qualified Data.HashMap.Strict as H

import Prelude hiding (Left,Right)



-- Euler 28: spiral int matrix



data Direction = Down | Left | Right | Up deriving Show



move :: (Int,Int) -> Direction -> (Int,Int)

move oldPoint@(y,x) dir =

  case dir of Down -> (y+1,x)

              Left -> (y,x-1)

              Right -> (y,x+1)

              Up -> (y-1,x)



getSpiralMoves :: Int -> [Direction]

getSpiralMoves n =

  if n == 3

  then [Down, Right, Up, Up, Left, Left, Down, Down]

  else let prevMoves = getSpiralMoves (n-2)

           firstMove = [Down]

           secondMove = take (n-2) $ repeat Right

           thirdMove = take (n-1) $ repeat Up

           fourthMove = take (n-1) $ repeat Left

           fifthMove = take (n-1) $ repeat Down

       in

        prevMoves ++ firstMove ++ secondMove ++ thirdMove ++ fourthMove ++ fifthMove



getSpiralCoords :: Int -> [(Int,Int)]

getSpiralCoords n =

  let mid = div (n+1) 2

      start = (mid,mid)

  in

   L.scanl move start $ getSpiralMoves n



getCoordHash :: Int -> H.HashMap (Int,Int) Int

getCoordHash n = H.fromList $ zip (getSpiralCoords n) [1..]



genF :: H.HashMap (Int,Int) Int -> (Int, Int) -> Int

genF h (i,j) = h H.! (i,j)



getDiagElems :: M.Matrix Int -> Int -> Int

getDiagElems mat n =

  let leftDiag = sum $ [M.getElem i i mat | i <- [1 .. n]]

      rightDiag = sum $ [M.getElem i (n + 1 - i) mat | i <- [1 .. n]]

      centerElem = let c = div (n+1) 2

                   in

                    M.getElem c c mat

  in

   leftDiag + rightDiag - centerElem



getSpiralMatrix :: Int -> M.Matrix Int

getSpiralMatrix n = M.matrix n n $ genF $ getCoordHash n



diagSum :: Int -> Int

diagSum n = getDiagElems (getSpiralMatrix n) n

```



I added type signatures for every function, so here's a quick overview (this is terribly over-engineered, and turned out to be more of a way to get to know various Haskell datatypes than to actually solve this problem!):



`getSpiralMoves` translates the literal pattern of the square spiral into concrete steps and then `getSpiralCoords` converts these into `(i,j)` elements within the matrix. Since I use a _generating function_ (in `getSpiralMatrix`) to create the matrix, I use a hash-map to store the value of each co-ordinate ... and then `getDiagElems` iterates over the diagonal elements.



The matrix does turn out as expected:



```haskell

*Main> getSpiralMatrix 7

( 43 42 41 40 39 38 37 )

( 44 21 20 19 18 17 36 )

( 45 22  7  6  5 16 35 )

( 46 23  8  1  4 15 34 )

( 47 24  9  2  3 14 33 )

( 48 25 10 11 12 13 32 )

( 49 26 27 28 29 30 31 )

```



This was a simple problem but I was surprised by how _short_ the code was; I didn't write overly terse code, added lots of whitespace, indentation, extra lines, etc, and the whole piece was _still_ just 60 lines!



**Update**: Fine, here's the simple solution too.



For every `N x N` matrix (where `N` is odd), the diagonal elements are all the diagonal elements of the `N-2`-sized matrix, plus four more. And these four include `N^2` and three others, each being `(N-1)` less than the other. The base case is `1 x 1`, with a value of 1.



```haskell

euler28 :: Int -> Int

euler28 1 = 1

euler28 n = let sideDiff = n - 1

                sq = n ^ 2

            in

             euler28 (n-2) + 4 * sq - (1 + 2 + 3) * sideDiff

```



... and then `euler28 1001` gives the same answer.
