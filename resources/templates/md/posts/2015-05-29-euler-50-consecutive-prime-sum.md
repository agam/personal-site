{:title "euler 50 consecutive prime sum"
:layout :post
 :tags ["old-post"]}



I had posted a grotesque non-working solution to this [previously](/blog/2015/05/17/euler-50-a-non-working-solution/), so here is a grotesque _working_ solution.



Yes, it uses global variables and old-fashioned extra book-keeping, and the `main` function is just a step-wise procedural function, but it _works_ and it's _correct_, so I'm going to leave it as is.



**Statutory warning**: _Spoilers ahead_



```go

const MAX_NUM = 1000000



type Number struct {

	isPrime              bool

	maxPrimeSum          int

	maxConsecutivePrimes int

	previousPrimeIndex   int

}



// Oh noez! A _global_ variable!

var numbers []Number



func markMultiples(p int) {

	for n := 2 * p; n < len(numbers); n += p {

		numbers[n].isPrime = false

	}

}



func populatePrimes() {

	// First mark all numbers as prime

	for i := 0; i < len(numbers); i++ {

		numbers[i].isPrime = true

	}



	// Bootstrap our loop with the first prime, 2

	numbers[0].isPrime = false

	numbers[1].isPrime = false



	prevPrime := 2

	numbers[2].isPrime = true

	numbers[2].previousPrimeIndex = -1



	// Straightforward sieve

	markMultiples(prevPrime)

	for p := 3; p < len(numbers); p++ {

		if numbers[p].isPrime {

			numbers[p].previousPrimeIndex = prevPrime

			prevPrime = p

			markMultiples(prevPrime)

		}

	}

}



func calculatePrimeSums() {

	// Initialize first prime, then move out from there

	numbers[2].maxPrimeSum = 2

	numbers[2].maxConsecutivePrimes = 1



	// As an upper limit for sums, what's the largest prime we have?

	largestPrime := MAX_NUM

	for i := MAX_NUM - 1; i > 0; i-- {

		if numbers[i].isPrime {

			largestPrime = i

			break

		}

	}

	fmt.Println("The largest possible prime to sum to is : ", largestPrime)



	for i := 3; i < len(numbers); i++ {

		if !numbers[i].isPrime {

			continue

		}



		sum := 0

		numPrimesTried := 0

		lastPrimeSum := 0

		numPrimesInSum := 0

		// Go back through the sequence of primes, until the

		// sum goes past the largest possible prime. Store the

		// last sum reached that _was_ a prime, and the number

		// of primes involved.

		for p := i; p > 0; p = numbers[p].previousPrimeIndex {

			if !numbers[p].isPrime {

				panic("wtf")

			}

			sum += p

			numPrimesTried++

			if sum > largestPrime {

				// Don't try any more primes!

				break

			} else {

				// Book keeping

				if numbers[sum].isPrime {

					lastPrimeSum = sum

					numPrimesInSum = numPrimesTried

				}

			}

		}

		numbers[i].maxPrimeSum = lastPrimeSum

		numbers[i].maxConsecutivePrimes = numPrimesInSum

		//		fmt.Println("max prime sum for ", i, " = ", numbers[i].maxPrimeSum)

	}

}



func findMaxPrimeSum() {

	// Use the pre-calculated primes sums to figure out the maximum and print some summary info

	maxConsecutivePrimes := 0

	maxPrimeSum := 0

	for i := 0; i < MAX_NUM; i++ {

		if numbers[i].isPrime {

			if numbers[i].maxConsecutivePrimes > maxConsecutivePrimes {

				maxConsecutivePrimes = numbers[i].maxConsecutivePrimes

				maxPrimeSum = numbers[i].maxPrimeSum

				fmt.Println("max prime sum = ", maxPrimeSum, ", with ", maxConsecutivePrimes, " primes, ending in ", i)

			}

		}

	}

}



func main() {

	numbers = make([]Number, MAX_NUM)

	populatePrimes()

	calculatePrimeSums()

	findMaxPrimeSum()

}

```



The best part? It ran in `0.363 seconds`!
