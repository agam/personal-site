{:title "a ridiculous graph"
:layout :post
 :tags ["old-post"]}



Following in the theme of the previous post, [here (see linked Gist)](https://gist.github.com/agam/fe8983acccd455fd9510) is a concurrent graph. Again, this is not an efficient way to do something like this, it's just a fun example of showing something that's easy in Go but probably harder or more verbose in other languages.



The basic idea here is that _every node is a goroutine_. So instead of using some structure like a matrix or an adjacency list to "get a handle" on all the nodes and their connections, you let the nodes take care of that themselves. Graph algorithms (trivial ones here are self-identification and depth calculation) are then constructed as interactions with the nodes, without an explicit traversal of vertices and edges.



```go

func node_agent(node *Node) {

	for {

		cmd := <-node.comm

		switch cmd.cmdtype {

		case NameCmd:

			fmt.Println("I am ", node.name)

		case PingCmd:

			cmd.cmdchan <- PingCmd

		case SetDepthCmd:

			// Ask all neighbors to modify depth, and pass

			// along the notification channel to them

			depth := cmd.cmdarg

			if node.state.depth > 0 {

				break

			}

			node.state.depth = depth

			for _, ch := range node.neighbors {

				ch <- Command{SetDepthCmd, cmd.cmdchan, depth + 1}

			}

			cmd.cmdchan <- 1

		case GetDepthCmd:

			cmd.cmdchan <- node.state.depth

		}

	}

}

```



The "Hello world" in this scenario is just finding out which nodes exist, which is something as follows:



```go

func IsAlive(node *Node) bool {

	// Ping the node with a 10ms timeout

	pingchan := make(chan int)

	node.comm <- Command{PingCmd, pingchan, 0}

	select {

	case <-pingchan:

		return true

	case <-time.After(10 * time.Millisecond):

		return false

	}

}



func printGraph(g *Graph) {

	fmt.Println("Graph :")



	for _, node := range g.nodes {

		if IsAlive(node) {

			node.comm <- Command{NameCmd, nil, 0}

		}

	}

}

```



A slightly more complicated operation is finding the depth of all nodes relative to the root node:



```go



func calculateDepths(g *Graph) {

	// Recursively trigger computation from root node

	notification := make(chan int)

	g.nodes["root"].comm <- Command{SetDepthCmd, notification, 1}



	for i := 0; i < len(g.nodes); i++ {

		// Wait for every node to receive this atleast once

		<-notification

	}



	// Now check all the depths

	depthchan := make(chan int)

	for name, node := range g.nodes {

		node.comm <- Command{GetDepthCmd, depthchan, 0}

		fmt.Println(name, " => ", <-depthchan)

	}

}

```



This being Go, you can play around with the implementation [in the playground](http://play.golang.org/p/SBoF_FnsMu)!
