{:title "battleships"
:layout :post
 :tags ["old-post"]}



All right, here you go, a first draft of a _dumb_ Battleships player.



```go

package main



import (

	"fmt"

	"log"

	"math/rand"

	"time"

	"unicode"

)



// "Classic" Battleship game, played as follows:

// -------------------------

// - Player gets a chance to select positions for ships

// - Player and computer take turns guessing a square on a grid

// - If the square corresponds to a ship, it is a hit;

//   if not, it is a miss

// - Whoever sinks the opponents' ships first, wins



// Internal representation:

// -------------------------

// Board size: 10 x 10

// 0 represents empty board

// 1 represents destroyer (2 units)

// 2 represents submarine (3 units)

// 3 represents cruiser (3 units)

// 4 represents battleship (4 units)

// 5 represents carrier (5 units)

// 6 represents a hit

// 7 represents a miss



const boardSize = 10



type direction int



const (

	up direction = iota

	down

	left

	right

)



const (

	hitValue  = 6

	missValue = 7

)



const (

	emptyStr = "   "

	shipStr  = " + "

	hitStr   = " X "

	missStr  = " . "

)



var shipSizes = map[int]int{

	1: 2, // destroyer

	2: 3, // submarine

	3: 3, // cruiser

	4: 4, // battleship

	5: 5, // carrier

}



type computerState struct {

	lastShotHit        bool

	lastShotSunk       bool

	lastHitRow         int

	lastHitColumn      int

	missesSinceLastHit int

}



// TODO: fold this into the map above

func getShipName(ship int) string {

	switch ship {

	case 1:

		return "Destroyer"

	case 2:

		return "Submarine"

	case 3:

		return "Cruiser"

	case 4:

		return "Battleship"

	case 5:

		return "Aircraft Carrier"

	default:

		log.Fatalf("Unknown ship! : %d", ship)

	}

	return "" // unfortunately, this is needed

}



func printBoard(board [][]int, shipsVisible bool) {

	fmt.Println("    0  1  2  3  4  5  6  7  8  9 ")

	for i := 0; i < boardSize; i++ {

		fmt.Printf(" %c ", i+'A')

		for j := 0; j < boardSize; j++ {

			switch board[i][j] {

			case 0:

				fmt.Printf(emptyStr)

			case 6:

				fmt.Printf(hitStr)

			case 7:

				fmt.Printf(missStr)

			case 1, 2, 3, 4, 5:

				if shipsVisible {

					fmt.Printf(shipStr)

				} else {

					fmt.Printf(emptyStr)

				}

			default:

				log.Fatalf("invalid board element: %d", board[i][j])

			}

		}

		fmt.Println()

	}

}



func makeBoard() [][]int {

	board := make([][]int, boardSize)

	for i := 0; i < boardSize; i++ {

		board[i] = make([]int, boardSize)

	}

	return board

}



func getTargetLocation(opponentBoard [][]int) (int, int) {

	for {

		fmt.Printf("\nChoose a grid position to hit : ")

		var choiceRow rune

		var col int

		fmt.Scanf("%c", &choiceRow)

		fmt.Scan(&col)



		row := int(unicode.ToLower(choiceRow) - 'a')



		if row < 0 || row > boardSize-1 || col < 0 || col > boardSize-1 {

			fmt.Println("Please enter a row from A to J, and a column from 1 to 10")

			continue

		}



		square := opponentBoard[row][col]

		if square == hitValue || square == missValue {

			fmt.Println("You already guessed this location! Try again ...")

			continue

		}



		// Ok, we got a valid square

		return row, col

	}

}



// TODO: slices are passed by reference, we don't need pointers!

func updateShot(opponentBoard [][]int, y, x int) {

	square := opponentBoard[y][x]

	if square == 0 {

		opponentBoard[y][x] = missValue

	} else {

		opponentBoard[y][x] = hitValue

	}

}



func scanBoardForShip(board [][]int, ship int) (sum int) {

	for i := 0; i < boardSize; i++ {

		for j := 0; j < boardSize; j++ {

			if board[i][j] == ship {

				sum++

			}

		}

	}

	return

}



func handlePlayerMove(computerBoard [][]int) bool {

	fmt.Println("\nHere is the computer's board so far: \n")

	printBoard(computerBoard, false)

	y, x := getTargetLocation(computerBoard)



	// Give the user some feedback

	square := computerBoard[y][x]

	if square == 0 {

		fmt.Println("\nYou missed")

	} else {

		// it has to be a ship, since a previous hit/miss is an invalid choice

		fmt.Println("\nYou scored a hit!")

		fmt.Printf("You hit a %s\n", getShipName(square))

	}



	updateShot(computerBoard, y, x)



	// Let the user know if a complete ship was sunk

	if scanBoardForShip(computerBoard, square) == 0 {

		fmt.Printf("You sunk the enemy's %s!\n", getShipName(square))

	}



	return !shipsLeft(computerBoard)

}



func shipsLeft(board [][]int) bool {

	for ship, _ := range shipSizes {

		if scanBoardForShip(board, ship) > 0 {

			return true

		}

	}

	return false

}



func findNewGuess(board [][]int) (int, int) {

	for {

		randRow := rand.Intn(boardSize)

		randCol := rand.Intn(boardSize)

		square := board[randRow][randCol]

		if square != hitValue && square != missValue {

			return randRow, randCol

		}

	}

}



func findAdjacentGuess(board [][]int, row, col int) (int, int) {

	// Try random directions

	for {

		randDir := direction(rand.Intn(4))

		if newRow, newCol, ok := getNextSquare(row, col, randDir); ok {

			square := board[newRow][newCol]

			if square != hitValue && square != missValue {

				return newRow, newCol

			}

		}

	}

}



func showGuess(row, col int) string {

	return fmt.Sprintf("%c%d", row+'A', col)

}



func handleComputerMove(playerBoard [][]int, state *computerState) bool {

	// The following (dumb!) algorithm is used:

	// - If the last shot was not a hit, guess randomly

	// - If the last shot sunk a ship, guess randomly

	// - If the last shot was a hit, then pick a new square "next to it"

	// - At the end, update state

	var row, col int

	if !state.lastShotHit || state.lastShotSunk {

		row, col = findNewGuess(playerBoard)

	} else {

		row, col = findAdjacentGuess(

			playerBoard, state.lastHitRow, state.lastHitColumn)

	}

	fmt.Printf("\nThe computer guessed: %s\n", showGuess(row, col))

	square := playerBoard[row][col]

	if square == 0 {

		fmt.Println("\nIt missed.")

		// Keep the last 'hit' sticky for a few rounds

		if state.lastShotHit {

			if state.missesSinceLastHit < 3 {

				state.missesSinceLastHit++

			} else {

				state.lastShotHit = false

				state.missesSinceLastHit = 0

			}

		}

	} else {

		fmt.Printf("It hit your %s!\n", getShipName(square))

		state.lastShotHit = true

		state.missesSinceLastHit = 0

		state.lastHitRow = row

		state.lastHitColumn = col

	}

	updateShot(playerBoard, row, col)

	if scanBoardForShip(playerBoard, square) == 0 {

		fmt.Printf("It sunk your %s!\n", getShipName(square))

		state.lastShotSunk = true

	} else {

		state.lastShotSunk = false

	}

	return !shipsLeft(playerBoard)

}



func addPlayerShips(board [][]int) {

	fmt.Println(

		"Begin adding ships. For each ship, enter the row and " +

			"column separated by a space, followed by another space " +

			"and then the direction to place it in.\n\n" +

			"For directions, use the following numeric codes: " +

			"1 = facing up, 2 = facing down, 3 = facing left, " +

			"4 = facing right\n" +

			"E.g. to place a submarine at E7 facing left, enter " +

			"E 7 3\n\n" +

			"This will result in it occupying squares E7, E6, E5.\n\n")

	for ship, size := range shipSizes {

		fmt.Println("This is your board so far: ")

		printBoard(board, true)

		for {

			fmt.Printf("\nEnter row, column, direction for your %s (occupying %d squares): ", getShipName(ship), size)

			var row, col, dir int

			var rowInput rune

			fmt.Scanf("%c", &rowInput)

			fmt.Scan(&col)

			fmt.Scan(&dir)

			row = int(unicode.ToLower(rowInput) - 'a')

			if placeShip(board, row, col, direction(dir-1), ship, size) {

				break

			} else {

				fmt.Println("Sorry, that doesn't seem to be a valid arrangement, please try again!\n")

			}

		}

	}

}



func getNextSquare(row, col int, d direction) (int, int, bool) {

	// TODO: Investigate possibile duplicate code between this

	// function and showShip() above

	switch d {

	case up:

		if row > 0 {

			return row - 1, col, true

		}

	case down:

		if row < boardSize-1 {

			return row + 1, col, true

		}

	case left:

		if col > 0 {

			return row, col - 1, true

		}

	case right:

		if col < boardSize-1 {

			return row, col + 1, true

		}

	}

	return row, col, false

}



func placeShip(board [][]int, row, col int, d direction, ship, size int) bool {

	if row < 0 || row > boardSize-1 || col < 0 || col > boardSize-1 {

		return false

	}

	// First check the starting square

	if board[row][col] != 0 {

		return false

	}



	// Place the given ship, of the given size, inf the board

	// starting from the given row and column, along the given

	// direction

	origRow, origCol := row, col

	ok := false

	for i := 0; i < size-1; i++ {

		row, col, ok = getNextSquare(row, col, d)

		if !ok {

			return false

		}

		if board[row][col] != 0 {

			return false

		}

	}

	// Ok, now actually change the board

	row, col = origRow, origCol

	for i := 0; i < size; i++ {

		board[row][col] = ship

		row, col, _ = getNextSquare(row, col, d)

	}

	return true

}



func addComputerShips(board [][]int) {

	// TODO: try placing ships in descending order of size

	for ship, size := range shipSizes {

		for {

			randRow := rand.Intn(boardSize)

			randCol := rand.Intn(boardSize)

			randDirection := direction(rand.Intn(4))

			if placeShip(board, randRow, randCol, randDirection, ship, size) {

				break

			}

		}

	}

}



func showUsage() {

	fmt.Println("\n      Welcome to BATTLESHIP !!   ")

	fmt.Println("----------------------------------------\n")

	fmt.Printf("First, you place your ships, then I place mine, and then we take turns guessing squares on each other's board, until one of us sinks the other's ships.\n")

	fmt.Println("\n     *** GOOD LUCK *** \n")

}



func main() {

	// Initialize boards for player and

	rand.Seed(time.Now().Unix())

	showUsage()

	playerBoard := makeBoard()

	computerBoard := makeBoard()



	addPlayerShips(playerBoard)

	addComputerShips(computerBoard)



	var state computerState



	victory := false

	defeat := false

	for {

		fmt.Println("\nHere is your board so far: \n")

		printBoard(playerBoard, true)

		victory = handlePlayerMove(computerBoard)

		if victory {

			fmt.Println("======== YOU WON !!! =======")

			break

		}

		defeat = handleComputerMove(playerBoard, &state)

		if defeat {

			fmt.Println("------ You Lost :( -------")

			break

		}

	}

}

```



As always, the corresponding BASIC code:



```basic

1000 PRINT TAB(33);"SALVO"

1010 PRINT TAB(15);"CREATIVE COMPUTING  MORRISTOWN, NEW JERSEY"

1020 PRINT:PRINT:PRINT

1030 REM

1040 DIM A(10,10),B(10,10),C(7),D(7),E(12),F(12),G(12),H(12),K(10,10)

1050 Z8=0

1060 FOR W=1 TO 12

1070 E(W)=-1

1080 H(W)=-1

1090 NEXT W

1100 FOR X=1 TO 10

1110 FOR Y=1 TO 10

1120 B(X,Y)=0

1130 NEXT Y

1140 NEXT X

1150 FOR X=1 TO 12

1160 F(X)=0

1170 G(X)=0

1180 NEXT X

1190 FOR X=1 TO 10

1200 FOR Y=1 TO 10

1210 A(X,Y)=0

1220 NEXT Y

1230 NEXT X

1240 FOR K=4 TO 1 STEP -1

1250 U6=0

1260 GOSUB 2910

1270 DEF FNA(K)=(5-K)*3-2*INT(K/4)+SGN(K-1)-1

1280 DEF FNB(K)=K+INT(K/4)-SGN(K-1)

1290 IF V+V2+V*V2=0 THEN 1260

1300 IF Y+V*FNB(K)>10 THEN 1260

1310 IF Y+V*FNB(K)<1 THEN 1260

1320 IF X+V2*FNB(K)>10 THEN 1260

1330 IF X+V2*FNB(K)<1 THEN 1260

1340 U6=U6+1

1350 IF U6>25 THEN 1190

1360 FOR Z=0 TO FNB(K)

1370 F(Z+FNA(K))=X+V2*Z

1380 G(Z+FNA(K))=Y+V*Z

1390 NEXT Z

1400 U8=FNA(K)

1405 IF U8>U8+FNB(K) THEN 1460

1410 FOR Z2= U8 TO U8+FNB(K)

1415 IF U8<2 THEN 1450

1420 FOR Z3=1 TO U8-1

1430 IF SQR((F(Z3)-F(Z2))^2 + (G(Z3)-G(Z2))^2) < 3.59 THEN 1260

1440 NEXT Z3

1450 NEXT Z2

1460 FOR Z=0 TO FNB(K)

1470 A(F(Z+U8),G(Z+U8))=.5+SGN(K-1)*(K-1.5)

1480 NEXT Z

1490 NEXT K

1500 PRINT "ENTER COORDINATES FOR..."

1510 PRINT "BATTLESHIP"

1520 FOR X=1 TO 5

1530 INPUT Y,Z

1540 B(Y,Z)=3

1550 NEXT X

1560 PRINT "CRUISER"

1570 FOR X=1 TO 3

1580 INPUT Y,Z

1590 B(Y,Z)=2

1600 NEXT X

1610 PRINT "DESTROYER<A>"

1620 FOR X=1 TO 2

1630 INPUT Y,Z

1640 B(Y,Z)=1

1650 NEXT X

1660 PRINT "DESTROYER<B>"

1670 FOR X=1 TO 2

1680 INPUT Y,Z

1690 B(Y,Z)=.5

1700 NEXT X

1710 PRINT "DO YOU WANT TO START";

1720 INPUT J$

1730 IF J$<>"WHERE ARE YOUR SHIPS?" THEN 1890

1740 PRINT "BATTLESHIP"

1750 FOR Z=1 TO 5

1760 PRINT F(Z);G(Z)

1770 NEXT Z

1780 PRINT "CRUISER"

1790 PRINT F(6);G(6)

1800 PRINT F(7);G(7)

1810 PRINT F(8);G(8)

1820 PRINT "DESTROYER<A>"

1830 PRINT F(9);G(9)

1840 PRINT F(10);G(10)

1850 PRINT "DESTROYER<B>"

1860 PRINT F(11);G(11)

1870 PRINT F(12);G(12)

1880 GOTO 1710

1890 C=0

1900 PRINT "DO YOU WANT TO SEE MY SHOTS";

1910 INPUT K$

1920 PRINT

1930 IF J$<>"YES" THEN 2620

1940 REM*******************START

1950 IF J$<>"YES" THEN 1990

1960 C=C+1

1970 PRINT

1980 PRINT "TURN";C

1990 A=0

2000 FOR W=.5 TO 3 STEP .5

2010 FOR X=1 TO 10

2020 FOR Y=1 TO 10

2030 IF B(X,Y)=W THEN 2070

2040 NEXT Y

2050 NEXT X

2060 GOTO 2080

2070 A=A+INT(W+.5)

2080 NEXT W

2090 FOR W=1 TO 7

2100 C(W)=0

2110 D(W)=0

2120 F(W)=0

2130 G(W)=0

2140 NEXT W

2150 P3=0

2160 FOR X=1 TO 10

2170 FOR Y=1 TO 10

2180 IF A(X,Y)>10 THEN 2200

2190 P3=P3+1

2200 NEXT Y

2210 NEXT X

2220 PRINT "YOU HAVE";A;"SHOTS."

2230 IF P3>=A THEN 2260

2240 PRINT "YOU HAVE MORE SHOTS THAN THERE ARE BLANK SQUARES."

2250 GOTO 2890

2260 IF A<>0 THEN 2290

2270 PRINT "I HAVE WON."

2280 STOP

2290 FOR W=1 TO A

2300 INPUT X,Y

2310 IF X<>INT(X) THEN 2370

2320 IF X>10 THEN 2370

2330 IF X<1 THEN 2370

2340 IF Y<>INT(Y) THEN 2370

2350 IF Y>10 THEN 2370

2360 IF Y>=1 THEN 2390

2370 PRINT "ILLEGAL, ENTER AGAIN."

2380 GOTO 2300

2390 IF A(X,Y)>10 THEN 2440

2400 C(W)=X

2410 D(W)=Y

2420 NEXT W

2430 GOTO 2460

2440 PRINT "YOU SHOT THERE BEFORE ON TURN";A(X,Y)-10

2450 GOTO 2300

2460 FOR W=1 TO A

2470 IF A(C(W),D(W))=3 THEN 2540

2480 IF A(C(W),D(W))=2 THEN 2560

2490 IF A(C(W),D(W))=1 THEN 2580

2500 IF A(C(W),D(W))=.5 THEN 2600

2510 A(C(W),D(W))=10+C

2520 NEXT W

2530 GOTO 2620

2540 PRINT "YOU HIT MY BATTLESHIP."

2550 GOTO 2510

2560 PRINT "YOU HIT MY CRUISER."

2570 GOTO 2510

2580 PRINT "YOU HIT MY DESTROYER<A>."

2590 GOTO 2510

2600 PRINT "YOU HIT MY DESTROYER<B>."

2610 GOTO 2510

2620 A=0

2630 IF J$="YES" THEN 2670

2640 C=C+1

2650 PRINT

2660 PRINT "TURN";C

2670 A=0

2680 FOR W=.5 TO 3 STEP .5

2690 FOR X=1 TO 10

2700 FOR Y=1 TO 10

2710 IF A(X,Y)=W THEN 2750

2720 NEXT Y

2730 NEXT X

2740 GOTO 2760

2750 A=A+INT(W+.5)

2760 NEXT W

2770 P3=0

2780 FOR X=1 TO 10

2790 FOR Y=1 TO 10

2800 IF A(X,Y)>10 THEN 2820

2810 P3=P3+1

2820 NEXT Y

2830 NEXT X

2840 PRINT "I HAVE";A;"SHOTS."

2850 IF P3>A THEN 2880

2860 PRINT "I HAVE MORE SHOTS THAN BLANK SQUARES."

2870 GOTO 2270

2880 IF A<>0 THEN 2960

2890 PRINT "YOU HAVE WON."

2900 STOP

2910 X=INT(RND(1)*10+1)

2920 Y=INT(RND(1)*10+1)

2930 V=INT(3*RND(1)-1)

2940 V2=INT(3*RND(1)-1)

2950 RETURN

2960 FOR W=1 TO 12

2970 IF H(W)>0 THEN 3800

2980 NEXT W

2990 REM*******************RANDOM

3000 W=0

3010 R3=0

3020 GOSUB 2910

3030 RESTORE

3040 R2=0

3050 R3=R3+1

3060 IF R3>100 THEN 3010

3070 IF X>10 THEN 3110

3080 IF X>0 THEN 3120

3090 X=1+INT(RND(1)*2.5)

3100 GOTO 3120

3110 X=10-INT(RND(1)*2.5)

3120 IF Y>10 THEN 3160

3130 IF Y>0 THEN 3270

3140 Y=1+INT(RND(1)*2.5)

3150 GOTO 3270

3160 Y=10-INT(RND(1)*2.5)

3170 GOTO 3270

3180 F(W)=X

3190 G(W)=Y

3200 IF W=A THEN 3380

3210 IF R2=6 THEN 3030

3220 READ X1,Y1

3230 R2=R2+1

3240 DATA 1,1,-1,1,1,-3,1,1,0,2,-1,1

3250 X=X+X1

3260 Y=Y+Y1

3270 IF X>10 THEN 3210

3280 IF X<1 THEN 3210

3290 IF Y>10 THEN 3210

3300 IF Y<1 THEN 3210

3310 IF B(X,Y)>10 THEN 3210

3320 FOR Q9=1 TO W

3330 IF F(Q9)<>X THEN 3350

3340 IF G(Q9)=Y THEN 3210

3350 NEXT Q9

3360 W=W+1

3370 GOTO 3180

3380 IF K$<>"YES" THEN 3420

3390 FOR Z5=1 TO A

3400 PRINT F(Z5);G(Z5)

3410 NEXT Z5

3420 FOR W=1 TO A

3430 IF B(F(W),G(W))=3 THEN 3500

3440 IF B(F(W),G(W))=2 THEN 3520

3450 IF B(F(W),G(W))=1 THEN 3560

3460 IF B(F(W),G(W))=.5 THEN 3540

3470 B(F(W),G(W))=10+C

3480 NEXT W

3490 GOTO 1950

3500 PRINT "I HIT YOUR BATTLESHIP"

3510 GOTO 3570

3520 PRINT "I HIT YOUR CRUISER"

3530 GOTO 3570

3540 PRINT "I HIT YOUR DESTROYER<B>"

3550 GOTO 3570

3560 PRINT "I HIT YOUR DESTROYER<A>"

3570 FOR Q=1 TO 12

3580 IF E(Q)<>-1 THEN 3730

3590 E(Q)=10+C

3600 H(Q)=B(F(W),G(W))

3610 M3=0

3620 FOR M2=1 TO 12

3630 IF H(M2)<>H(Q) THEN 3650

3640 M3=M3+1

3650 NEXT M2

3660 IF M3<>INT(H(Q)+.5)+1+INT(INT(H(Q)+.5)/3) THEN 3470

3670 FOR M2=1 TO 12

3680 IF H(M2)<>H(Q) THEN 3710

3690 E(M2)=-1

3700 H(M2)=-1

3710 NEXT M2

3720 GOTO 3470

3730 NEXT Q

3740 PRINT "PROGRAM ABORT:"

3750 FOR Q=1 TO 12

3760 PRINT "E(";Q;") =";E(Q)

3770 PRINT "H(";Q;") =";H(Q)

3780 NEXT Q

3790 STOP

3800 REM************************USINGEARRAY

3810 FOR R=1 TO 10

3820 FOR S=1 TO 10

3830 K(R,S)=0

3840 NEXT S

3850 NEXT R

3860 FOR U=1 TO 12

3870 IF E(U)<10 THEN 4020

3880 FOR R=1 TO 10

3890 FOR S=1 TO 10

3900 IF B(R,S)<10 THEN 3930

3910 K(R,S)=-10000000

3920 GOTO 4000

3930 FOR M=SGN(1-R) TO SGN(10-R)

3940 FOR N=SGN(1-S) TO SGN(10-S)

3950 IF N+M+N*M=0 THEN 3980

3960 IF B(R+M,S+N)<>E(U) THEN 3980

3970 K(R,S)=K(R,S)+E(U)-S*INT(H(U)+.5)

3980 NEXT N

3990 NEXT M

4000 NEXT S

4010 NEXT R

4020 NEXT U

4030 FOR R=1 TO A

4040 F(R)=R

4050 G(R)=R

4060 NEXT R

4070 FOR R=1 TO 10

4080 FOR S=1 TO 10

4090 Q9=1

4100 FOR M=1 TO A

4110 IF K(F(M),G(M))>=K(F(Q9),G(Q9)) THEN 4130

4120 Q9=M

4130 NEXT M

4131 IF R>A THEN 4140

4132 IF R=S THEN 4210

4140 IF K(R,S)<K(F(Q9),G(Q9)) THEN 4210

4150 FOR M=1 TO A

4160 IF F(M)<>R THEN 4190

4170 IF G(M)=S THEN 4210

4180 NEXT M

4190 F(Q9)=R

4200 G(Q9)=S

4210 NEXT S

4220 NEXT R

4230 GOTO 3380

4240 END

```



And finally (drumroll!) some gameplay:



(aren't you glad _BASIC_ isn't pushed as a "first programming language" any more?!)



```sh

$ go run battleship.go



Welcome to BATTLESHIP !!

----------------------------------------



First, you place your ships, then I place mine, and then we take turns guessing squares on each other's board, until one of us sinks the other's ships.



*** GOOD LUCK ***



Begin adding ships. For each ship, enter the row and column separated by a space, followed by another space and then the direction to place it in.



For directions, use the following numeric codes: 1 = facing up, 2 = facing down, 3 = facing left, 4 = facing right

E.g. to place a submarine at E7 facing left, enter E 7 3



This will result in it occupying squares E7, E6, E5.



This is your board so far:

0  1  2  3  4  5  6  7  8  9

A

B

C

D

E

F

G

H

I

J



Enter row, column, direction for your Battleship (occupying 4 squares): B 3 2

This is your board so far:

0  1  2  3  4  5  6  7  8  9

A

B           +

C           +

D           +

E           +

F

G

H

I

J



Enter row, column, direction for your Aircraft Carrier (occupying 5 squares): I 2 4

This is your board so far:

0  1  2  3  4  5  6  7  8  9

A

B           +

C           +

D           +

E           +

F

G

H

I        +  +  +  +  +

J



Enter row, column, direction for your Destroyer (occupying 2 squares): C 8 2

This is your board so far:

0  1  2  3  4  5  6  7  8  9

A

B           +

C           +              +

D           +              +

E           +

F

G

H

I        +  +  +  +  +

J



Enter row, column, direction for your Submarine (occupying 3 squares): F 5 4

This is your board so far:

0  1  2  3  4  5  6  7  8  9

A

B           +

C           +              +

D           +              +

E           +

F                 +  +  +

G

H

I        +  +  +  +  +

J





Enter row, column, direction for your Cruiser (occupying 3 squares): E 0 2



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +              +

D           +              +

E  +        +

F  +              +  +  +

G  +

H

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C

D

E

F

G

H

I

J



Choose a grid position to hit : C 4



You missed



The computer guessed: H6



It missed.





Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +              +

D           +              +

E  +        +

F  +              +  +  +

G  +

H                    .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C              .

D

E

F

G

H

I

J



Choose a grid position to hit : H 7



You missed



The computer guessed: D4



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +              +

D           +  .           +

E  +        +

F  +              +  +  +

G  +

H                    .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C              .

D

E

F

G

H                       .

I

J



Choose a grid position to hit : G2



You missed



The computer guessed: C7



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +           .  +

D           +  .           +

E  +        +

F  +              +  +  +

G  +

H                    .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C              .

D

E

F

G        .

H                       .

I

J



Choose a grid position to hit : F 7



You missed



The computer guessed: C9



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +           .  +  .

D           +  .           +

E  +        +

F  +              +  +  +

G  +

H                    .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C              .

D

E

F                       .

G        .

H                       .

I

J



Choose a grid position to hit : E7



You missed



The computer guessed: D1



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +           .  +  .

D     .     +  .           +

E  +        +

F  +              +  +  +

G  +

H                    .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C              .

D

E                       .

F                       .

G        .

H                       .

I

J



Choose a grid position to hit : C3



You missed



The computer guessed: H2



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A

B           +

C           +           .  +  .

D     .     +  .           +

E  +        +

F  +              +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B

C           .  .

D

E                       .

F                       .

G        .

H                       .

I

J



Choose a grid position to hit : B 3



You missed



The computer guessed: A0



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A  .

B           +

C           +           .  +  .

D     .     +  .           +

E  +        +

F  +              +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B           .

C           .  .

D

E                       .

F                       .

G        .

H                       .

I

J



Choose a grid position to hit : E 2



You missed



The computer guessed: E2



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A  .

B           +

C           +           .  +  .

D     .     +  .           +

E  +     .  +

F  +              +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B           .

C           .  .

D

E        .              .

F                       .

G        .

H                       .

I

J



Choose a grid position to hit : G 5



You missed



The computer guessed: E1



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A  .

B           +

C           +           .  +  .

D     .     +  .           +

E  +  .  .  +

F  +              +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B           .

C           .  .

D

E        .              .

F                       .

G        .        .

H                       .

I

J



Choose a grid position to hit : C 8



You missed



The computer guessed: F0

It hit your Cruiser!



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A  .

B           +

C           +           .  +  .

D     .     +  .           +

E  +  .  .  +

F  X              +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B           .

C           .  .           .

D

E        .              .

F                       .

G        .        .

H                       .

I

J



Choose a grid position to hit : I 4



You missed



The computer guessed: F1



It missed.



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A  .

B           +

C           +           .  +  .

D     .     +  .           +

E  +  .  .  +

F  X  .           +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B           .

C           .  .           .

D

E        .              .

F                       .

G        .        .

H                       .

I              .

J





Choose a grid position to hit : C6



You scored a hit!

You hit a Battleship



The computer guessed: E0

It hit your Cruiser!



Here is your board so far:



0  1  2  3  4  5  6  7  8  9

A  .

B           +

C           +           .  +  .

D     .     +  .           +

E  X  .  .  +

F  X  .           +  +  +

G  +

H        .           .

I        +  +  +  +  +

J



Here is the computer's board so far:



0  1  2  3  4  5  6  7  8  9

A

B           .

C           .  .     X     .

D

E        .              .

F                       .

G        .        .

H                       .

I              .

J



<... and so on ...>

```
