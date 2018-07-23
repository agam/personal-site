{:title "simple subtraction dot dot dot and gambling"
:layout :post
 :tags ["old-post"]}



Continuing on the theme of the last two posts, here are two more examples, here are _two more_. The first is a simple change-counting program, which might fit the bill for a "first program", and the second is a game of craps (not too interactive, unfortunately; all you do is place a bet -- I did at least add a delay for the dice throw to make it seem less ... er ... automated).



### Change: Go Source



```go

package main



import (

	"fmt"

	"math"

	"strings"

)



type moneyUnit struct {

	name  string

	value int

}



var moneyUnits = []moneyUnit{

	{"Hundred Dollar Bill", 100 * 100},

	{"Fifty Dollar Bill", 50 * 100},

	{"Twenty Dollar Bill", 20 * 100},

	{"Ten Dollar Bill", 10 * 100},

	{"Five Dollar Bill", 5 * 100},

	{"One Dollar Bill", 100},

	{"One-Half Dollar", 50},

	{"Quarter", 25},

	{"Dime", 10},

	{"Nickel", 5},

	{"Penny", 1},

}



func showUsage() {

	fmt.Println("\n         The Change-Maker   ")

	fmt.Println("----------------------------------\n")

	fmt.Println("I'll help you calculate the change for any item purchased.\n")

}



func getMoney(prompt string) float64 {

	for {

		fmt.Printf("%s : $", prompt)

		var answer float64

		fmt.Scan(&answer)

		pennies := answer * 100

		roundPennies := math.Ceil(pennies)

		if roundPennies-pennies > 1e-4 {

			fmt.Println("Error: please enter $XXXX.XX (i.e. up to two decimal places!)")

		} else {

			return answer

		}

	}

}



func deductChange(pennies, unit int) (numUnits int, remaining int) {

	if pennies%unit == 0 {

		return pennies / unit, 0

	} else {

		numUnits = pennies / unit

		remaining = pennies - numUnits*unit

		return

	}

}



func showChange(pennies int) {

	for _, u := range moneyUnits {

		var numUnits int

		numUnits, pennies = deductChange(pennies, u.value)

		if numUnits > 0 {

			fmt.Printf("%5d  %s(s)\n", numUnits, u.name)

		}

	}

	if pennies != 0 {

		panic("wtf")

	}

}



func main() {

	showUsage()

	for {

		cost := getMoney("Cost of item?")

		payment := getMoney("Amount of payment?")

		if payment == cost {

			fmt.Println("Aha! you don't need any change!\n")

		} else if payment-cost > 0 {

			showChange(int((payment - cost) * 100))

		} else {

			fmt.Println("\nHey! you need to pay me some more for that!")

		}

		fmt.Printf("\nAnother item? (enter 'y' or 'n'): ")

		var another string

		fmt.Scan(&another)

		if strings.ToLower(another) == "n" {

			break

		} else {

			fmt.Println()

		}

	}

	fmt.Println("\n\nThank you, come again!\n\n")

}

```



### Change: Basic Source



```basic

2 PRINT TAB(33);"CHANGE"

4 PRINT TAB(15);"CREATIVE COMPUTING  MORRISTOWN, NEW JERSEY"

5 PRINT:PRINT:PRINT

6 PRINT "I, YOUR FRIENDLY MICROCOMPUTER, WILL DETERMINE"

8 PRINT "THE CORRECT CHANGE FOR ITEMS COSTING UP TO $100."

9 PRINT:PRINT

10 PRINT "COST OF ITEM";:INPUT A:PRINT "AMOUNT OF PAYMENT";:INPUT P

20 C=P-A:M=C:IF C<>0 THEN 90

25 PRINT "CORRECT AMOUNT, THANK YOU."

30 GOTO 400

90 IF C>0 THEN 120

95 PRINT "SORRY, YOU HAVE SHORT-CHANGED ME $";A-P

100 GOTO 10

120 PRINT "YOUR CHANGE, $";C

130 D=INT(C/10)

140 IF D=0 THEN 155

150 PRINT D;"TEN DOLLAR BILL(S)"

155 C=M-(D*10)

160 E=INT(C/5)

170 IF E=0 THEN 185

180 PRINT E;"FIVE DOLLARS BILL(S)"

185 C=M-(D*10+E*5)

190 F=INT(C)

200 IF F=0 THEN 215

210 PRINT F;"ONE DOLLAR BILL(S)"

215 C=M-(D*10+E*5+F)

220 C=C*100

225 N=C

230 G=INT(C/50)

240 IF G=0 THEN 255

250 PRINT G;"ONE HALF DOLLAR(S)"

255 C=N-(G*50)

260 H=INT(C/25)

270 IF H=0 THEN 285

280 PRINT H;"QUARTER(S)"

285 C=N-(G*50+H*25)

290 I=INT(C/10)

300 IF I=0 THEN 315

310 PRINT I;"DIME(S)"

315 C=N-(G*50+H*25+I*10)

320 J=INT(C/5)

330 IF J=0 THEN 345

340 PRINT J;"NICKEL(S)"

345 C=N-(G*50+H*25+I*10+J*5)

350 K=INT(C+.5)

360 IF K=0 THEN 380

370 PRINT K;"PENNY(S)"

380 PRINT "THANK YOU, COME AGAIN."

390 PRINT:PRINT

400 GOTO 10

410 END

```



### Change: Gameplay



```sh

$ go run change.go



         The Change-Maker

		 ----------------------------------



I'll help you calculate the change for any item purchased.



Cost of item? : $4.56

Amount of payment? : $20

1  Ten Dollar Bill(s)

1  Five Dollar Bill(s)

1  Quarter(s)

1  Dime(s)

1  Nickel(s)

4  Penny(s)



Another item? (enter 'y' or 'n'): y



Cost of item? : $56.43

Amount of payment? : $525.50

4  Hundred Dollar Bill(s)

1  Fifty Dollar Bill(s)

1  Ten Dollar Bill(s)

1  Five Dollar Bill(s)

4  One Dollar Bill(s)

1  Nickel(s)

2  Penny(s)



Another item? (enter 'y' or 'n'): n





Thank you, come again!

```



### Craps: Go Source



```go

package main



import (

	"fmt"

	"math/rand"

	"strings"

	"time"

)



const (

	playerStartingMoney = 100

	minimumBetAmount    = 5

	diceThrowDelayMs    = 300

	pointBreakThrow     = 7

)



const (

	rollerPlayer = iota

	rollerComputer

)



const (

	winStr       = "... and Win!"

	loseStr      = "... and Lose!"

	rollAgainStr = "... roll again ..."

)



var winningThrows = map[int]bool{

	7:  true,

	11: true,

}



var losingThrows = map[int]bool{

	2:  true,

	3:  true,

	12: true,

}



func showUsage() {

	fmt.Printf("See rules? enter 'y' or 'n' : ")

	var choice string

	fmt.Scan(&choice)

	if strings.ToLower(choice) == "n" {

		return

	}

	fmt.Println("\n       Welcome to CRAPS!    ")

	fmt.Println("----------------------------------------")

	fmt.Println("\nHere are the rules:")

	fmt.Println("- You roll a pair of dice and add up the score for each turn")

	fmt.Println("- rolling a 7 or 11 on the first roll wins")

	fmt.Println("- rolling a 2, 3 or 12 on the first roll loses")

	fmt.Println("- any other number becomes your 'point', and you continue to roll")

	fmt.Println("    - if you subsequently roll your 'point' again, you win")

	fmt.Println("    - if you roll a 7, you lose")

	fmt.Println("- When one of us loses, we switch dice")

	fmt.Println("- You start with $100, and have to bet a minimum of $5 per turn")

	fmt.Println("- You get the first turn")

	fmt.Println("- To stop betting, just enter $0 as your bet amount")

	fmt.Println("\n\nThat's it, let's go!\n\n")

}



func getBetMoney(total int) (bet int) {

	for {

		fmt.Printf("\nYou have $%d, how much would you like to bet? $", total)

		fmt.Scan(&bet)

		if bet > total {

			fmt.Println("Hey, you can't bet more than you have!")

		} else if bet > 0 && bet < minimumBetAmount {

			fmt.Printf("Sorry buddy, you gotta place at least $%d on the table.\n", minimumBetAmount)

		} else {

			return

		}

	}

}



func getDice(subject string) int {

	fmt.Printf("%s roll ", subject)

	time.Sleep(diceThrowDelayMs * time.Millisecond)

	dice1 := 1 + rand.Intn(6)

	fmt.Printf("%d ", dice1)

	time.Sleep(diceThrowDelayMs * time.Millisecond)

	dice2 := 1 + rand.Intn(6)

	fmt.Printf("and %d ", dice2)

	time.Sleep(diceThrowDelayMs * time.Millisecond)

	return dice1 + dice2

}



func playRound(roller int) bool {

	subject := "I"

	possessive := "My"

	if roller == rollerPlayer {

		subject = "You"

		possessive = "Your"

	}



	firstThrow := getDice(subject)



	if winningThrows[firstThrow] {

		fmt.Println(winStr)

		return true

	}

	if losingThrows[firstThrow] {

		fmt.Println(loseStr)

		return false

	}



	fmt.Printf("so %s point is %d\n", possessive, firstThrow)

	for {

		throw := getDice(subject)

		if throw == firstThrow {

			fmt.Printf("and make %s point!\n", possessive)

			return true

		}

		if throw == pointBreakThrow {

			fmt.Println(loseStr)

			return false

		}

		fmt.Println(rollAgainStr)

	}

}



func updateMoney(won bool, total, bet int) (newTotal int) {

	if won {

		newTotal = total + bet

	} else {

		newTotal = total - bet

	}

	return

}



func showQuitMessage() {

	fmt.Println("\nAh I wish I could quit this easily ... farewell, young gambler!\n")

}



func main() {

	showUsage()

	rand.Seed(time.Now().Unix())



	playerMoney := playerStartingMoney

	for {

		fmt.Println("\n\nOk, your turn now\n")

		for {

			bet := getBetMoney(playerMoney)

			if bet == 0 {

				showQuitMessage()

				return

			}

			playerWon := playRound(rollerPlayer)

			playerMoney = updateMoney(playerWon, playerMoney, bet)

			if !playerWon {

				break

			}

		}



		fmt.Println("\n\nOk, my turn now\n")

		for {

			bet := getBetMoney(playerMoney)

			if bet == 0 {

				showQuitMessage()

				return

			}

			computerWon := playRound(rollerComputer)

			playerMoney = updateMoney(!computerWon, playerMoney, bet)

			if !computerWon {

				break

			}

		}

	}

}

```



### Craps: Basic Source



```basic

5 PRINT TAB(33);"CRAPS"

10 PRINT TAB(15);"CREATIVE COMPUTING  MORRISTOWN, NEW JERSEY"

12 PRINT:PRINT:PRINT

15 LET R=0

20 PRINT"2,3,12 ARE LOSERS; 4,5,6,8,9,10 ARE POINTS; 7,11 ARE NATURAL WINNERS."

21 LET T=1

22 PRINT "PICK A NUMBER AND INPUT TO ROLL DICE";

23 INPUT Z

24 LET X=(RND(0))

25 LET T =T+1

26 IF T<=Z GOTO 24

27 PRINT"INPUT THE AMOUNT OF YOUR WAGER.";

28 INPUT F

30 PRINT "I WILL NOW THROW THE DICE"

40 LET E=INT(7*RND(1))

41 LET S=INT(7*RND(1))

42 LET X=E+S

50 IF X=7 GOTO 180

55 IF X=11 GOTO 180

60 IF X=1 GOTO 40

62 IF X=2 GOTO 195

65 IF X=0 GOTO 40

70 IF X=2 GOTO 200

80 IF X=3 GOTO 200

90 IF X=12 GOTO 200

125 IF X=5 GOTO 220

130 IF X =6 GOTO 220

140 IF X=8 GOTO 220

150 IF X=9 GOTO 220

160 IF X =10 GOTO 220

170 IF X=4 GOTO 220

180 PRINT X "- NATURAL....A WINNER!!!!"

185 PRINT X"PAYS EVEN MONEY, YOU WIN"F"DOLLARS"

190 GOTO 210

195 PRINT X"- SNAKE EYES....YOU LOSE."

196 PRINT "YOU LOSE"F "DOLLARS."

197 LET F=0-F

198 GOTO 210

200 PRINT X " - CRAPS...YOU LOSE."

205 PRINT "YOU LOSE"F"DOLLARS."

206 LET F=0-F

210 LET R= R+F

211 GOTO 320

220 PRINT X "IS THE POINT. I WILL ROLL AGAIN"

230 LET H=INT(7*RND(1))

231 LET Q=INT(7*RND(1))

232 LET O=H+Q

240 IF O=1 GOTO 230

250 IF O=7 GOTO 290

255 IF O=0 GOTO 230

260 IF O=X GOTO 310

270 PRINT O " - NO POINT. I WILL ROLL AGAIN"

280 GOTO 230

290 PRINT O "- CRAPS. YOU LOSE."

291 PRINT "YOU LOSE $"F

292 F=0-F

293 GOTO 210

300 GOTO 320

310 PRINT X"- A WINNER.........CONGRATS!!!!!!!!"

311 PRINT X "AT 2 TO 1 ODDS PAYS YOU...LET ME SEE..."2*F"DOLLARS"

312 LET F=2*F

313 GOTO 210

320 PRINT " IF YOU WANT TO PLAY AGAIN PRINT 5 IF NOT PRINT 2";

330 INPUT M

331 IF R<0 GOTO 334

332 IF R>0 GOTO 336

333 IF R=0 GOTO 338

334 PRINT "YOU ARE NOW UNDER $";-R

335 GOTO 340

336 PRINT "YOU ARE NOW AHEAD $";R

337 GOTO 340

338 PRINT "YOU ARE NOW EVEN AT 0"

340 IF M=5 GOTO 27

341 IF R<0 GOTO 350

342 IF R>0 GOTO 353

343 IF R=0 GOTO 355

350 PRINT"TOO BAD, YOU ARE IN THE HOLE. COME AGAIN."

351 GOTO 360

353 PRINT"CONGRATULATIONS---YOU CAME OUT A WINNER. COME AGAIN!"

354 GOTO 360

355 PRINT"CONGRATULATIONS---YOU CAME OUT EVEN, NOT BAD FOR AN AMATEUR"

360 END

```



### Craps: Gameplay



```sh

$ go run craps.go

See rules? enter 'y' or 'n' : y



       Welcome to CRAPS!

	   ----------------------------------------



Here are the rules:

- You roll a pair of dice and add up the score for each turn

- rolling a 7 or 11 on the first roll wins

- rolling a 2, 3 or 12 on the first roll loses

- any other number becomes your 'point', and you continue to roll

    - if you subsequently roll your 'point' again, you win

    - if you roll a 7, you lose

- When one of us loses, we switch dice

- You start with $100, and have to bet a minimum of $5 per turn

- You get the first turn

- To stop betting, just enter $0 as your bet amount





That's it, let's go!



Ok, your turn now



You have $100, how much would you like to bet? $10

You roll 2 and 3 so Your point is 5

You roll 6 and 5 ... roll again ...

You roll 1 and 5 ... roll again ...

You roll 4 and 6 ... roll again ...

You roll 6 and 5 ... roll again ...

You roll 1 and 5 ... roll again ...

You roll 2 and 2 ... roll again ...

You roll 2 and 3 and make Your point!



You have $110, how much would you like to bet? $20

You roll 3 and 2 so Your point is 5

You roll 2 and 4 ... roll again ...

You roll 6 and 4 ... roll again ...

You roll 3 and 1 ... roll again ...

You roll 4 and 3 ... and Lose!



Ok, my turn now



You have $90, how much would you like to bet? $0



Ah I wish I could quit this easily ... farewell, young gambler!



```



Coming next: a different kind of gambling game (Blackjack?) and a slightly more involved two-player game (Battleship?)






