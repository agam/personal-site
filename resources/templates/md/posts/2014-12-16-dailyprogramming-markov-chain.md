{:title "dailyprogramming markov chain"
:layout :post
 :tags ["old-post"]}



Been a long, _long_ time since I looked at [r/dailyprogrammer](http://www.reddit.com/r/dailyprogrammer/), so I picked [this _intermediate_ problem](http://www.reddit.com/r/dailyprogrammer/comments/2ovt2i/20141210_challenge_192_intermediate_markov_chain/) from 5 days ago. Basically, given a dictionary, judge the likelihood of a word being a "real word".



Here's my solution:



```go

package main



import (

	"bufio"

	"flag"

	"fmt"

	"log"

	"math"

	"os"

)



type wordmap map[uint8]map[uint8]int



func MakeWordMap(w string, wm wordmap) {

	for i := range w {

		if i == len(w)-1 {

			break

		}

		c := w[i]

		cnext := w[i+1]

		if _, ok := wm[c]; !ok {

			wm[c] = make(map[uint8]int)

		}

		wm[c][cnext] = wm[c][cnext] + 1

	}

}



func ReadWords(dictfile string, wm wordmap) {

	file, err := os.Open(dictfile)

	if err != nil {

		log.Fatal(err)

	}

	defer file.Close()



	scanner := bufio.NewScanner(file)

	for scanner.Scan() {

		MakeWordMap(scanner.Text(), wm)

	}

}



func CheckValidity(w string, wm wordmap) float64 {

	var c, cprev uint8

	var wordScore float64 = 1.0

	for i := range w {

		if i == 0 {

			cprev = w[i]

			continue

		}

		c = w[i]

		pairScore := wm[cprev][c]

		rowTotal := 0

		for _, count := range wm[cprev] {

			rowTotal += count

		}

		wordScore = wordScore * float64(pairScore) / float64(rowTotal)

		cprev = c

	}

	return wordScore

}



func main() {

	// Takes a dictionary and a single word, and returns the

	// probability of that word being "good"

	wm := make(wordmap)

	dictfile := flag.String("dictfile", "", "Path to word list")

	checkword := flag.String("checkword", "", "Word to spell-check")

	flag.Parse()

	ReadWords(*dictfile, wm)

	prob := CheckValidity(*checkword, wm)

	fmt.Printf("%s has a score of %.20f\n", *checkword, prob)

	if prob < math.Pow10(-len(*checkword)) {

		fmt.Println("It is probably not a word!")

	} else {

		fmt.Println("It looks like a real word!")

	}

}

```



It runs as follows:



```sh

$ go run markov.go -dictfile=/usr/share/dict/american-english  -checkword=go

go has a score of 0.06318184860451107887

It looks like a real word!

```
