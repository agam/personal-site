{:title "euler 47 distinct prime factors"
:layout :post
 :tags ["old-post"]}



This one turned out to be very similar to the previous one (except for the small detail of iterating from the smallest prime upwards, instead of the other way round).



**Statutory Warning**: _Spoilers ahead_



```cpp

class PrimeIterator {

public:

  PrimeIterator() : prime_index_(-1) {}



  int getNextPrime() {

    assert((prime_index_ + 1) < primes_.size());

    return primes_[++prime_index_];

  }



  bool hasMorePrimes() {

    assert(prime_index_ >= -1 && (prime_index_ + 1) <= primes_.size());

    return (prime_index_ + 1) < primes_.size();

  }



  void fillPrimes(int max) {

    while (primes_.back() <= max) {

      addPrime();

    }

    // Reset to before the first element

    prime_index_ = -1;

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



int getNumPrimeFactors(int n) {

  int numPrimeFactors = 0;

  PrimeIterator pit;

  pit.fillPrimes(n);

  while (n > 1 && pit.hasMorePrimes()) {

    int p = pit.getNextPrime();

    if (n % p == 0) {

      while (n % p == 0) {

        n /= p;

      }

      ++numPrimeFactors;

    }

  }

  return numPrimeFactors;

}



int main() {

  cout << "Euler #47 ... \n";



  int runLength = 0;

  for (int n = 2; ; ++n) {

    if (getNumPrimeFactors(n) == 4) {

      ++runLength;

    } else {

      runLength = 0;

    }

    if (runLength == 4) {

      cout << "Found a sequence of four numbers with four prime "

           << "factors starting at : " << n - 3 << endl;

      return 0;

    }

  }

}

```



Runs as follows:



```sh

$ time ./Test

Euler #47 ...

Found a sequence of four numbers with four prime factors starting at : <redacted>



real    0m3.814s

user    0m3.812s

sys     0m0.000s

```



(Yep, a noticeable delay, but anything under 5 seconds is good for me ...)
