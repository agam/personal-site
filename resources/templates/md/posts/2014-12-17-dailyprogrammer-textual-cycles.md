{:title "dailyprogrammer textual cycles"
:layout :post
 :tags ["old-post"]}



[Another one](http://www.reddit.com/r/dailyprogrammer/comments/2m82yz/20141114_challenge_188_hard_arrows_and_arrows/) from [r/dailyprogrammer](http://www.reddit.com/r/dailyprogrammer). This one involves text with "arrows", but as the "hint" at the end says, it's really equivalent to finding cycles in a directed graph (it would be harder if there were multiple edges going out of each node, but that isn't the case here!)



Here's my code:



```go

package main



import "fmt"



type Node struct {

	val     uint8

	next    *Node

	visited bool

	cycle   int

}



func main() {

	var height, width int

	fmt.Scanln(&width, &height)

	grid := make([]Node, height*width)

	for i := 0; i < height; i++ {

		var str string

		fmt.Scanln(&str)

		for j := range str {

			grid[i*width+j] = Node{str[j], nil, false, 1}

		}

	}



	CreateLinks(&grid, height, width)



	// Traverse graph; each node is visited at most once,

	// so the algorithm is O(n^2)

	longestCycle := 0

	var rootx, rooty int

	for i := 0; i < height; i++ {

		for j := 0; j < width; j++ {

			curNode := &grid[i*width+j]

			if curNode.visited {

				continue

			}

			curCycleLength := TraverseNode(curNode)

			if curCycleLength > longestCycle {

				rooty, rootx = i, j

				longestCycle = curCycleLength

			}

		}

	}

	fmt.Printf("The largest cycle was found at (%d, %d) with a length of %d\n", rooty, rootx, grid[rooty*width+rootx].cycle-1)

	TraceCycle(grid, height, width, rooty, rootx)

}



func TraverseNode(node *Node) int {

	// Recursively returns the length of the cycle found starting

	// at the current node

	if node.visited {

		return node.cycle

	}

	node.visited = true

	cycleLength := 1 + TraverseNode(node.next)

	node.cycle = cycleLength

	return cycleLength

}



func GetNextNode(val uint8, i, j, h, w int) (int, int) {

	switch val {

	case '<':

		return i, (j + w - 1) % w

	case '>':

		return i, (j + 1) % w

	case '^':

		return (i + h - 1) % h, j

	case 'v':

		return (i + 1) % h, j

	default:

		panic("wtf")

	}

}



func CreateLinks(grid *[]Node, height, width int) {

	var node *Node

	for i := 0; i < height; i++ {

		for j := 0; j < width; j++ {

			node = &(*grid)[i*width+j]

			nexti, nextj := GetNextNode(node.val, i, j, height, width)

			node.next = &(*grid)[nexti*width+nextj]

		}

	}

}



func TraceCycle(grid []Node, height, width, curY, curX int) {

	tracedGrid := make([]Node, height*width)

	for i := 0; i < height; i++ {

		for j := 0; j < width; j++ {

			tracedGrid[i*width+j] = Node{' ', nil, false, 1}

		}

	}



	for {

		v := grid[curY*width+curX].val

		tracedGrid[curY*width+curX].val = v

		tracedGrid[curY*width+curX].visited = true

		curY, curX = GetNextNode(v, curY, curX, height, width)



		if tracedGrid[curY*width+curX].visited == true {

			break

		}

	}



	fmt.Println("Here is the longest cycle :-\n")

	for i := 0; i < height; i++ {

		for j := 0; j < width; j++ {

			fmt.Printf("%c", tracedGrid[i*width+j].val)

		}

		fmt.Println()

	}

}

```



It runs as follows:



(sample 1)



```

$ cat sample-input1

5 5

>>>>v

^v<<v

^vv^v

^>>v<

^<<<^

```



```sh

$ cat sample-input1 | go run cycles.go                                                                                                                       

The largest cycle was found at (0, 0) with a length of 16

Here is the longest cycle :-



>>>>v

^   v

^   v

^  v<

^<<<

```



(sample 2)



```

45 20

^^v>>v^>>v<<<v>v<>>>>>>>>^vvv^^vvvv<v^^><^^v>

>><<>vv<><<<^><^<^v^^<vv>>^v<v^vv^^v<><^>><v<

vv<^v<v<v<vvv>v<v<vv<^<v<<<<<<<<^<><>^><^v>>>

<v<v^^<v<>v<>v<v<^v^>^<^<<v>^v><^v^>>^^^<><^v

^>>>^v^v^<>>vvv>v^^<^<<<><>v>>^v<^^<>v>>v<v>^

^^^<<^<^>>^v>>>>><>>^v<^^^<^^v^v<^<v^><<^<<<>

v<>v^vv^v<><^>v^vv>^^v^<>v^^^>^>vv<^<<v^<<>^v

<<<<<^<vv<^><>^^>>>^^^^<^<^v^><^v^v>^vvv>^v^^

<<v^<v<<^^v<>v>v^<<<<<>^^v<v^>>>v^><v^v<v^^^<

^^>>^<vv<vv<>v^<^<^^><><^vvvv<<v<^<<^>^>vv^<v

^^v^>>^>^<vv^^<>>^^v>v>>v>>v^vv<vv^>><>>v<<>>

^v<^v<v>^^<>>^>^>^^v>v<<<<<>><><^v<^^v><v>^<<

v>v<><^v<<^^<^>v>^><^><v^><v^^^>><^^<^vv^^^>^

v><>^><vv^v^^>><>^<^v<^><v>^v^<^<>>^<^vv<v>^v

><^<v>>v>^<<^>^<^^>v^^v<>>v><<>v<<^><<>^>^v<v

>vv>^>^v><^^<v^>^>v<^v><>vv>v<^><<<<v^<^vv<>v

<><<^^>>^<>vv><^^<vv<<^v^v^<^^^^vv<<>^<vvv^vv

>v<<v^><v<^^><^v^<<<>^<<vvvv^^^v<<v>vv>^>>^<>

^^^^<^<>^^vvv>v^<<>><^<<v>^<<v>>><>>><<^^>vv>

<^<^<>vvv^v><<<vvv<>>>>^<<<^vvv>^<<<^vv>v^><^

```



```sh

$ cat sample-input2 | go run cycles.go

The largest cycle was found at (19, 18) with a length of 49

Here is the longest cycle :-



                 >>>>>>>>^

                    ^<

                     ^

                    >^

                    ^

                   >^

                   ^

                >>>^

                ^

                ^<

                 ^

                 ^

                 ^

                >^

                ^

                ^

                ^  v<<

                ^<<< ^

                     ^<<

                 v<    ^<<

```



**Notes:**



- Spent a lot of time tracking down essentially a `s/width/height` error throughout. Embarrassing.



- I'm a bit disturbed that my answer for sample 2 is different from the one given in the problem description (that one is _shorter_!)
