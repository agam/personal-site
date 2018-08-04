{:title "On Theorem proving and formal verification"
:layout :post
:tags ["typed"]}

I keep dipping my toes into “dependent-types”, never going all the way. Came across [this long and interesting post](https://www.hillelwayne.com/post/theorem-prover-showdown/) on various languages that take different approaches here, with different tradeoffs (either being more focussed on theorem proving, or being more like “regular” programming languages, etc.)

One of the items was a comparison of how _leftpad_ would be written/verified in each of these, so even though I can’t read/understand these yet, bookmarking the for future reference.

**Agda:**
<script src="https://gist.github.com/rntz/aaaaf7a0bdb1cc65c49adb6adde29728.js"></script>

**Idris:**
<script src="https://gist.github.com/porglezomp/e941da088e462bc76a5080a46f904834.js"></script>

**Liquid Haskell:**
[https://github.com/hwayne/lets-prove-leftpad/blob/master/liquidhaskell/LeftPad.hs](https://github.com/hwayne/lets-prove-leftpad/blob/master/liquidhaskell/LeftPad.hs)

