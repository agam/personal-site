{:title "euler 46 goldbacks other conjecture"
:layout :post
 :tags ["old-post"]}



May the brute force be with you. Here is another one (sorry for the over-engineered code).



**Statutory Warning**: _Spoilers ahead_



```c++

class PrimeIterator {

public:

  PrimeIterator() : prime_index_(primes_.size() - 1) {}



  int getNextPrime() {

    assert(prime_index_ > 0);

    return primes_[--prime_index_];

  }



  bool hasMorePrimes() {

    return prime_index_ > 0;

  }



  void fillPrimes(int max) {

    while (primes_.back() <= max) {

      addPrime();

    }

    // Reset to the last element

    prime_index_ = primes_.size() - 1;

  }



private:

  static void addPrime() {

    assert(primes_.size() > 0);

    int n = primes_.back();

    while (true) {

      ++n;

      bool is_prime = true;

      for (const int& p : primes_) {

        if (n % p == 0) {

          is_prime = false;

          break;  // Try next number

        }

      }

      if (is_prime) {

        primes_.push_back(n);

        return;

      }

    }

  }



  static vector<int> primes_;

  

  int prime_index_;

};



vector<int> PrimeIterator::primes_ = {2};



bool goldbachDecompositionExists(int n) {

  assert(n % 2 == 1);

  // n = p + 2*m^2

  // Since 2*m^2 is always even, and n is odd, p must be odd too (so p has to be 3 or greater)



  // Use a generator to get more primes on demand.

  PrimeIterator pit;

  pit.fillPrimes(n);

  while (pit.hasMorePrimes()) {

    int p = pit.getNextPrime();

    if (p == n) {

      // If n is a prime, then n = p + 2*(0*0)

      return true;

    }

    double sq = (n - p)/2.0;

    if (sqrt(sq) == floor(sqrt(sq))) {

      // n = p + 2*sqrt(sq)

      return true;

    }

  }

  return false;

}



int main() {

  cout << "Euler #46 ... \n";



  // Loop over odd composite numbers ...

  for (int i = 9; ; i+= 2) {

    if (!goldbachDecompositionExists(i)) {

      cout << "Failed at " << i << endl;

      return -1;

    }

  }

}

```



Runs as follows:



```sh

$ time ./Test

Euler #46 ...

Failed at <redacted>



real    0m0.007s

user    0m0.004s

sys     0m0.000s

```




