{:title "euler 50 a non working solution"
:layout :post
 :tags ["old-post"]}



It's worth pointing out things that don't work, roads that lead to dead ends, etc -- so here is a brute force solution that has absolutely no chance of ever working. It's long and verbose because it's a _franken-solution_, made out of parts of previous solution concatenated together into a terrible mess. Posting it here before I destroy it. The forlorn `TODO` at the end proved over-optimistic ...



```c++

static const int kMaxNum = 1000000;



void PopulatePrimes(vector<bool>* numbers, vector<int>* prime_indices) {

  // Start off with all numbers marked prime.

  for (int i = 2; i < numbers->size(); ++i) {

    numbers->at(i) = true;

  }

  

  int prime_index = 2;

  while (true) {

    // Store the prime index for future reference

    prime_indices->push_back(prime_index);



    if (numbers->size() < 2 * prime_index) {

      break;

    }



    // Mark the multiples as not prime

    for (int i = prime_index * 2; i < numbers->size(); i += prime_index) {

      numbers->at(i) = false;

    }



    // Repeat with the next prime number

    do {

      ++prime_index;

    } while (prime_index < numbers->size() && !numbers->at(prime_index));



    if (prime_index == numbers->size()) {

      break;

    }

  }

}



void sanity_check_primes(const vector<bool>& numbers,

                         const vector<int>& prime_indices) {

  auto check_prime = [&numbers, &prime_indices](int n, bool is_prime) {

    if (is_prime) {

      assert(numbers[n] == true);

      assert(std::find(prime_indices.begin(), prime_indices.end(), n) !=

             prime_indices.end());

    } else {

      assert(numbers[n] == false);

      assert(std::find(prime_indices.begin(), prime_indices.end(), n) ==

             prime_indices.end());

    }

  };



  check_prime(2, true);

  check_prime(3, true);

  check_prime(5, true);

  check_prime(4, false);

  check_prime(6, false);

}



class CombinationIterator {

public:

  CombinationIterator(int n, int m) : n_(n), m_(m) {

    comb_.reserve(m);

    for (int i = 0; i < m; ++i) {

      comb_.push_back(i);

    }

  }



  const vector<int>& GetCombination() const {

    return comb_;

  }



  bool Next() {

    // The last digit can go up to n, the next-to-last up to n-1, and

    // so on. The very first sequence is {0, 1, ..., m-1}, and the

    // very last is {n-m+1, ..., n-1, n}.

    for (int i = m_ - 1; i >= 0; --i) {

      int max = n_ + i - m_ + 1;

      int val = comb_[i];

      // cout << "Debug: i = " << i << ", val = " << val << ", max = " << max << endl;

      if (val < max) {

        // Increment this position, and update subsequent indices if

        // necessary.

        for (int j = i; j < m_; ++j) {

          comb_[j] = ++val;

        }

        return true;

      }



      // If we're at the beginning, we're done.

      if (i == 0) {

        return false;

      }



      // Otherwise, fallthrough to the previous index.

    }

    assert(false);  // Should not reach here!

  }

private:

  const int n_, m_;

  vector<int> comb_;

};



void sanity_check_combinator() {

  CombinationIterator cit(9, 4);

  for (int i = 0; i < 20; ++i) {

    const auto& v = cit.GetCombination();

    cout << "Debug (" << v.size() << ") : ";

    for (int n : v) {

      cout << n << "  ";

    }

    cout << endl;

    assert(cit.Next());

  }

}



enum class Cardinality {

  Zero, One, More

};



Cardinality GetPrimeSum(int num_summands,

                        const vector<bool>& numbers,

                        const vector<int>& prime_indices) {

  // Try all possible combinations of adding prime numbers

  // together. Either exhaust all combinations, in which case return

  // Zero or One. The moment two are found, return More.



  int num_primes = 0;

  CombinationIterator cit(numbers.size(), num_summands);

  do {

    const auto& pv = cit.GetCombination();

    int sum = 0;

    for (int p : pv) {

      sum += p;

    }



    // Check if the primes add up to a prime

    if (sum < numbers.size() && numbers[sum]) {

      ++num_primes;

    }

    if (num_primes == 2) {

      return Cardinality::More;

    }

  } while (cit.Next());

  assert(num_primes == 0 || num_primes == 1);

  if (num_primes == 1) {

    return Cardinality::One;

  } else {

    return Cardinality::Zero;

  }

}



std::ostream& operator<< (std::ostream& os, const Cardinality& c) {

  os << static_cast<std::underlying_type<Cardinality>::type>(c);

  return os;

}



int main() {

  cout << "Euler #50 ... \n";



  // Get the prime numbers upto 1 million

  vector<bool> numbers(kMaxNum);

  vector<int> prime_indices;

  PopulatePrimes(&numbers, &prime_indices);



  sanity_check_primes(numbers, prime_indices);



  cout << "Debug: number of primes  = " << prime_indices.size() << endl;

  sanity_check_combinator();



  cout << "Debug : " << GetPrimeSum(10000, numbers, prime_indices);

  // TODO(agam): Uncomment and continue ...

  // int left = 0;

  // int right = 500;  /// random round number

  // do {

  //   Cardinality left_c = GetPrimeSum(left, numbers, prime_indices);

  //   Cardinality right_c = GetPrimeSum(right, numbers, prime_indices);

  // } while (left < right);

}

```
