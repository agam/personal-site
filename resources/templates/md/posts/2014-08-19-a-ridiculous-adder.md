{:title "a ridiculous adder"
:layout :post
 :tags ["old-post"]}



Alright, the first example of _"Ridiculous Concurrency"_ ! Here is a terribly inefficient adder that performs arithmetic on arbitrarily large integers.



See [this gist](https://gist.github.com/agam/3d82745c84c8f44dd51c) for the complete source ...



In case no one gets the joke, I'll be explicit: this is _not really_ a way to implement a "BigNum", it's a terrible way to do it, but it's a fun way to do it that can only be done with Go.



This is the `main` function:



```go

func main() {

     bignum := MakeBigNum([]int{9, 9, 8})

     print(bignum)

     fmt.Println("\nAnd after adding ... \n")

 

     add(100000, bignum)

 

     print(bignum)

      fmt.Println()

}

```



Pretty straightforward ... and this is how you make the big number:



```go

func addDigit(b *Bignum, num int, in <-chan bool) chan bool {

     new_out := make(chan bool)

     d := &Digit{num, in, new_out}

     b.digits = append(b.digits, d)

     go carry(d)

     return new_out

}



func carry(d *Digit) {

         for in_msg := range d.pull_carry {

     	         out_msg := false

                 if in_msg {

                         sum := d.value + 1

                         if sum >= 10 {

                                 d.value = sum - 10

                                 out_msg = true

                         } else {

                                 d.value = sum

                         }

                 }

                 d.push_carry <- out_msg

          }

}



func sentinel(b *Bignum, in <-chan bool) {

        for {

                var msg = <-in

                if msg {

                        last_out := addDigit(b, 1, in)

                        go sentinel(b, last_out)

                        last_out <- false

                        break

                 } else {

                        b.stable <- true

                 }

         }

}

 

func MakeBigNum(num_arr []int) *Bignum {

         var digits []*Digit

         last_out := make(chan bool)

 

         stable := make(chan bool)

         b := &Bignum{digits, last_out, stable}

         l := len(num_arr)

 

         for i := 0; i < l; i++ {

                 last_out = addDigit(b, num_arr[l-i-1], last_out)

         }

 

         go sentinel(b, last_out)

 

         return b

}



```



Yup, _every digit_ is a Goroutine here!



Each digit is connected to the one on its left and the one on its right, and has channels open to each. It listens in on one (`pull_carry`) and increments itself if necessary. If it overflows, it lets the next one know.



The last digit is always a "sentinel", which on receiving a carry signal (i.e. when the overall big number grows in size), creates a new "digit" (spawning a goroutine for it) and makes _that_ one the new sentinel.



With this arrangement, the actual task of addition is boring:



```go

func add(n int, b *Bignum) {

        sent := 0

        done := 0

done_sending:

	for {

	         select {

	    	 case b.add <- true:

		         sent++

			 if sent == n {

			         break done_sending

			 }

 

	         case <-b.stable:

		         done++

		 }

	 }

	 for ; done < n; done++ {

	         <- b.stable

	 }

}

```



Well, alright, not _that_ boring. `b.add` is nothing other than the `pull_carry` channel of the _first_ digit. We just pump that channel as many times as the size of the number we're adding, and wait for the number to "stabilize" at the end. `b.stable` is the flip side of `b.add`: it's the `push_carry` channel of the _last_ digit.



The small trick here is to start receiving the results in parallel, to avoid deadlock. Another option would be to provide buffered channels, but I prefer it this way so I don't have to think about how much buffer space to allocate. This is just as correct.



That's pretty much it ... here are the data types we used in our adder:



```go

type Bignum struct {

         digits   []*Digit

         add      chan<- bool

         stable   chan bool

}

 

type Digit struct {

         value      int

         pull_carry <-chan bool

         push_carry chan<- bool

}



```



This isn't really about adding numbers, creating a reasonably complex (statically typed!) asynchronous model like this would be hard in any other language (yes, I'm aware of `core.async`, and it has `go`, `|>`, `<|` and so on, and using `alts!!` might result in something similar, but it feels a bit clunkier to play around with).



Here's another great perk of #golang ... you don't have to take my word for all this: try out this example live at [the Go Playground](http://play.golang.org/p/dOK16qov1e)!
