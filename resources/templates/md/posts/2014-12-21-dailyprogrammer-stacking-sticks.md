{:title "dailyprogrammer stacking sticks"
:layout :post
 :tags ["old-post"]}



Some of the most interesting (and frustrating!) problems are the ones which are _obvious_ to you as a human being but quite _unobvious_ how to put into code.



[This one](http://www.reddit.com/r/dailyprogrammer/comments/2oe0px/2014125_challenge_191_hard_tricky_stick_stacking/) from `r/dailyprogrammer` is one of those. Given "sticks" on a plane, how do you order them so that they can be slid across one axis without touching each other (assuming the sticks don't cross each other to begin with) ?



In this case that axis is the "positive-Y", and it should be possible to come up with a simpler answer (since this is sort of a 2-D depth buffer), but for now this is my messy answer:



```go

package main



import "fmt"

import "math"



type Stick struct {

	id             int

	x1, y1, x2, y2 float64

}



type CompareResult int



const (

	LessThan CompareResult = iota

	EqualTo

	GreaterThan

)



type Sticks []Stick



func (s1 Stick) Compare(s2 Stick) CompareResult {

	maxY1 := math.Max(s1.y1, s1.y2)

	minY1 := math.Min(s1.y1, s1.y2)

	maxY2 := math.Max(s2.y1, s2.y2)

	minY2 := math.Min(s2.y1, s2.y2)



	if minY2 > maxY1 {

		return LessThan

	}

	if minY1 > maxY2 {

		return GreaterThan

	}



	// Inconclusive, we need to consider the x-values too

	maxX1 := math.Max(s1.x1, s1.x2)

	minX1 := math.Min(s1.x1, s1.x2)

	maxX2 := math.Max(s2.x1, s2.x2)

	minX2 := math.Min(s2.x1, s2.x2)



	// The equality case : the lines should be "exclusive"

	// (otherwise, it means they are crossing!).

	if minX1 > maxX2 || minX2 > maxX1 {

		return EqualTo

	}



	// Alright, there is clearly overlap, so we look at a bunch of

	// cases.



	// First, is one line wholly contained within the other? If

	// yes, we only need to check one point; the "non-crossing"

	// condition takes care of it being true for all points.

	if maxX1 > maxX2 && minX1 < minX2 {

		if s2.getY(minX2) >= s1.getY(minX2) {

			return LessThan

		} else {

			return GreaterThan

		}

	}

	if maxX1 < maxX2 && minX1 > minX2 {

		if s2.getY(minX1) >= s1.getY(minX1) {

			return LessThan

		} else {

			return GreaterThan

		}

	}



	// Lines partially overlap; so there are two cases to consider

	if maxX1 >= minX2 && minX2 >= minX1 {

		// The "left" portion of s2 overlaps the "right" portion of s1

		if s2.getY(minX2) >= s1.getY(minX2) {

			return LessThan

		} else {

			return GreaterThan

		}

	}

	if maxX2 >= minX1 && maxX1 >= maxX2 {

		// vice-versa

		if s2.getY(maxX2) >= s1.getY(maxX2) {

			return LessThan

		} else {

			return GreaterThan

		}

	}



	// We should have handled all the cases!

	panic("Wtf!!")

}



func (s Stick) getY(x float64) float64 {

	// Special case for vertical line

	if s.x1 == s.x2 {

		if x == s.x1 {

			return s.y1

		} else {

			panic(fmt.Sprintf("Incorrect x = %f for stick %+v", x, s))

		}

	}

	// For all other cases, use the slope of the line

	slope := (s.y2 - s.y1) / (s.x2 - s.x1)

	return s.y1 + (slope * (x - s.x1))

}



func main() {

	var ss Sticks

	var numSticks int

	fmt.Scanln(&numSticks)

	for i := 0; i < numSticks; i++ {

		var id int

		var x1, y1, x2, y2 float64

		fmt.Scanf("%d:%f,%f,%f,%f\n",

			&id, &x1, &y1, &x2, &y2)

		if i != id-1 {

			panic("WTF")

		}

		s := Stick{id, x1, y1, x2, y2}

		ss = append(ss, s)

	}



	ss.TotalSort()



	fmt.Println("Here is the computed stick order :-\n")



	PrintSticks(ss)

}



func PrintSticks(ss Sticks) {

	for i, s := range ss {

		if i == len(ss)-1 {

			fmt.Println(s.id)

		} else {

			fmt.Printf("%d, ", s.id)

		}

	}

}



func (ss *Sticks) TotalSort() {

	for i := 0; i < len(*ss)-1; i++ {

		max := i

		for j := i + 1; j < len(*ss); j++ {

			if (*ss)[max].Compare((*ss)[j]) == LessThan {

				max = j

			}

		}

		if max != i {

			(*ss)[i], (*ss)[max] = (*ss)[max], (*ss)[i]

		}

	}

}

```



It runs as follows:



```sh

$ cat sample-input2

5

1:3,3,8,1

2:11,2,15,2

3:6,3,12,4

4:10,5,10,10

5:9,11,18,12



$ cat sample-input2 | go run sticks.go

Here is the computed stick order :-



5, 4, 3, 2, 1

```



Most of the "action" is in the `Compare()` function, but I also like how it's possible to define arbitrary "receivers" for functions in Go (e.g. used for `getY()` above)
