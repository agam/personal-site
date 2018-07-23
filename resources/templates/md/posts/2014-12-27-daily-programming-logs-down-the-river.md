{:title "daily programming logs down the river"
:layout :post
 :tags ["old-post"]}



Found [this problem](http://www.reddit.com/r/dailyprogrammer/comments/2lljyq/11052014_challenge_187_hard_lumberjack_floating/) from about a month ago. I did a sloppy job on this one -- it's correct, but mostly straight-line "C - style" code. I'll come up with a better way to do this later, _promise_. Meanwhile, here's the code.



```go

package main



import "bufio"

import "fmt"

import "os"



type nodelabel uint8



type link struct {

	next      *node

	available int

}



type node struct {

	label    nodelabel

	links    []link

	hops     int

	capacity int

	isFinal  bool

}



type nodeset map[nodelabel]*node



func main() {

	nodes := make(nodeset)

	scanner := bufio.NewScanner(os.Stdin)

	var numNodes int

	for scanner.Scan() {

		line := scanner.Text()

		if line[0] == '#' {

			continue

		}

		if numNodes == 0 {

			fmt.Sscanln(line, &numNodes)

		} else {

			var startNode, endNode nodelabel

			var capacity int

			fmt.Sscanf(line, "%c %c %d", &startNode, &endNode, &capacity)

			n := nodes.getNode(startNode)

			next := nodes.getNode(endNode)

			l := link{next: next, available: capacity}

			n.links = append(n.links, l)

		}

	}

	finalNodeLabel := nodelabel('A') + nodelabel(numNodes-1)



	startNode := nodes.getNode('A')

	numLogs := 0

	for {

		nodes.updateHopsAndCapacities(finalNodeLabel)



		if startNode.capacity == 0 {

			break

		}

		path := startNode.sendOneLog()

		numLogs++



		// Print out path (in reverse)

		fmt.Printf("Log #%d takes ", numLogs)

		for i := len(path) - 1; i > 0; i-- {

			fmt.Printf("%c -> ", path[i])

		}

		fmt.Printf("%c -- path of %d\n", path[0], len(path))

	}

	fmt.Printf("River is now full. We could send %d logs\n", numLogs)

}



func (nodes nodeset) updateHopsAndCapacities(finalNodeLabel nodelabel) {

	for _, n := range nodes {

		n.hops = -1

	}

	for _, n := range nodes {

		n.computeHops(finalNodeLabel)

		n.capacity = 0

		for _, l := range n.links {

			n.capacity += l.available

		}

	}

}



func (n *node) sendOneLog() []nodelabel {

	if n.isFinal {

		singlePath := make([]nodelabel, 1)

		singlePath[0] = n.label

		return singlePath

	}



	// Pick shortest path

	minHops := -1

	var nextLink *link

	for i := 0; i < len(n.links); i++ {

		l := &n.links[i]

		if !l.next.isFinal && l.next.capacity == 0 {

			continue

		}

		if minHops < 0 || (l.available > 0 && l.next.hops < minHops) {

			minHops = l.next.hops

			nextLink = l

		}

	}



	path := nextLink.next.sendOneLog()



	// Update current node and link

	n.capacity--

	nextLink.available--



	// TODO: avoid making a copy here (!)

	newPath := make([]nodelabel, len(path)+1)

	copy(newPath, path)

	newPath[len(path)] = n.label

	return newPath

}



func (nodes nodeset) getNode(r nodelabel) *node {

	n, ok := nodes[r]

	if !ok {

		n = &node{label: r, isFinal: false}

		nodes[r] = n

	}

	return n

}



func (n *node) computeHops(finish nodelabel) {

	if n.hops >= 0 {

		return

	}

	if n.label == finish {

		n.hops = 0

		n.isFinal = true

		return

	}

	minHops := -1

	for _, l := range n.links {

		l.next.computeHops(finish)

		if l.available == 0 {

			continue

		}

		if minHops < 0 ||

			l.next.hops < minHops {

			minHops = l.next.hops

		}

	}

	n.hops = minHops + 1

}

```



It was easy to read in a formatted text file, so I did that instead of storing the specified routes and weights as a data literal. Here is the input:



```sh

# Format:

# Number of nodes on the first line

# Each subsequent line contains

# <start node> <end node> capacity

# Nodes are labelled starting with 'A'

9

A B 6

A C 2

B E 3

B D 3

D C 2

D F 1

C G 5

E H 1

E I 2

F H 1

G H 2

G I 2

H I 4

```



And here is how the program runs:



```sh

$ cat /tmp/logroutes  | go run src/reddit-prog/log-routes.go



Log #1 takes A -> B -> E -> I -- path of 4

Log #2 takes A -> B -> E -> I -- path of 4

Log #3 takes A -> C -> G -> I -- path of 4

Log #4 takes A -> C -> G -> I -- path of 4

Log #5 takes A -> B -> E -> H -> I -- path of 5

Log #6 takes A -> B -> D -> F -> H -> I -- path of 6

Log #7 takes A -> B -> D -> C -> G -> H -> I -- path of 7

Log #8 takes A -> B -> D -> C -> G -> H -> I -- path of 7

River is now full. We could send 8 logs.

```
