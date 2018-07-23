{:title "euler 49 prime permutations"
:layout :post
 :tags ["old-post"]}



This one took way longer than expected. I initially misunderstood the question to require 4-digit numbers _unique digits_ (because that's what the example has!) and I went crazy trying to figure out why the _only_ answer I was getting was the example in the question. Anyway I later realized I'd over-complicated my solution and I just needed to look at numbers between `1000` and `9999`. Given this, it's _absurd_ to have a class called "FourDigits" (duh), but I figured there's no point hiding my initial mistake :)



The code fragment below is bloated because I've left in old code from my first attempt (the functions suffixed with `...Old`), and half the code here is dead code.



**Statutory Warning**: _Spoilers Ahead_ (well, not really)



```cpp

bool IsPrime(int n) {

  // Rule out even numbers

  if ((n > 2) && (n % 2 == 0)) return false;

  

  // Check divisibility by odd numbers starting from 3, uptil the

  // square root of the number.

  const int lim = sqrt(n);

  for (int i = 3; i <= lim; i+=2) {

    if (n % i == 0) {

      return false;

    }

  }

  return true;

}



bool HasArithmeticSequence(const vector<int>& v) {

  assert(v.size() >= 3);

  // Assumes sorted vector of ints

  struct diff {

    int n1, n2, d;

  };

  vector<diff> diffs;

  // Get the diffs between all elements, then find two pairs with the

  // same diff (yes, N^2, but will do). The two pairs must share a

  // number, i.e. V_i + d = V_j, and V_j + d = V_k.

  for (int i = 0; i < v.size() - 1; ++i) {

    for (int j = i+1; j < v.size(); ++j) {

      assert(v[j] > v[i]);

      diffs.push_back( { v[i], v[j], v[j] - v[i] } );

    }

  }

  std::sort(diffs.begin(), diffs.end(),

            [](const diff& df1, const diff& df2) {

              return df1.d < df2.d;

            });

  

  // Now that we have grouped elements by their difference, we can

  // analyze each 'cluster' to find the pairs we want.

  for (int i = 0; i < diffs.size() - 1; ++i) {

    // Yes, it's inefficient, but ...

    int d = diffs[i].d;

    for (int j = i + 1; diffs[j].d == d; ++j) {

      if (diffs[i].n2 == diffs[j].n1) {

        cout << "Found arithmetic progression: "

             << diffs[i].n1 << " -> "

             << diffs[i].n2 << " -> "

             << diffs[j].n2 << endl;

        return true;

      }

    }

  }

  return false;

}



class FourDigitCombinator {

public:

  FourDigitCombinator() : digits_({0,1,2,3}), number_(1000) {}



  string GetDigitsOld() {

    std::ostringstream stream;

    for (int i = 0; i < kNumDigits; ++i) {

      stream << digits_[i];

    }

    return stream.str();

  }



  bool NextOld() {

    sanity_check();

    // See if there is a prior number that can be incremented

    for (int i = kNumDigits - 1; i >= 0; --i) {

      // The last digit can go up to kMaxDigit, the previous one up to

      // kMaxDigit - 1, and so on ...

      const int digitMax = kMaxDigit - kNumDigits + i + 1;

      int digitValue = digits_[i];

      assert(digitValue <= digitMax);

      if (digitValue == digitMax) {

        // We've hit the limit for this digit. If this is the first

        // digit, we've reached the end.

        if (i == 0) {

          return false;

        }

        // Otherwise, fall through to the previous digit ...

      } else {

        // Increment, and reset subsequent digits, if any.

        for (int j = i; j < kNumDigits; ++j) {

          ++digitValue;

          digits_[j] = digitValue;

        }

        return true;

      }

    }

    assert(false);  // We should not get here!

  }

  

  bool HasPrimePermutationsOld() {

    array<int, kNumDigits> mutation = digits_;

    vector<int> prime_mutations;

    do {

      // Skip permutations with a leading zero.

      if (mutation[0] == 0) continue;

      

      // Create the corresponding number

      int number = 0;

      for (int i = 0; i < kNumDigits; ++i) {

        number = number * 10 + mutation[i];

      }

      if (IsPrime(number)) {

        prime_mutations.push_back(number);

      }

    } while (std::next_permutation(mutation.begin(), mutation.end()));



    if ((prime_mutations.size() >= 3) && HasArithmeticSequence(prime_mutations)) {

      return true;

    }

    return false;

  }

  

  string GetDigits() {

    std::ostringstream stream;

    stream << number_;

    return stream.str();

  }

  

  bool Next() {

    if (number_ < 9999) {

      ++number_;

      return true;

    }

    return false;

  }

  

  bool HasPrimePermutations() {

    array<int, kNumDigits> mutation;

    assert(number_ >= 1000 && number_ <= 9999);



    // Convert number into an array of digits ...

    int n = number_;

    int digit_index = kNumDigits - 1;

    while (n > 0) {

      mutation[digit_index--] = n % 10;

      n /= 10;

    }



    vector<int> prime_mutations;

    do {

      // ... then convert the array of digits back into a number!

      int num = 0;

      for (int i = 0; i < kNumDigits; ++i) {

        num = num * 10 + mutation[i];

      }

      assert(num >= 1000 && num <= 9999);

      if (IsPrime(num)) {

        prime_mutations.push_back(num);

      }

    } while (std::next_permutation(mutation.begin(), mutation.end()));



    if ((prime_mutations.size() >= 3) && HasArithmeticSequence(prime_mutations)) {

      return true;

    }

    return false;

  }



private:

  static const int kNumDigits = 4;

  static const int kMaxDigit = 9;

  array<int, kNumDigits> digits_;

  int number_;



  void sanity_check() {

    for (int i = 0; i < kNumDigits - 1; ++i) {

      assert(digits_[i] < digits_[i+1]);

    }

  }

};



int main() {

  cout << "Euler #49 ... \n";



  set<string> candidates;



  // Go over all sets of four digits, and consider the permutations of

  // each to see if any group of three permutations is prime.

  FourDigitCombinator four_digits;

  do {

    if (four_digits.HasPrimePermutations()) {

      candidates.insert(four_digits.GetDigits());

    }

  } while (four_digits.Next());



  // Debugging aid ... check if any combinations matched.

  for (const auto& c : candidates) {

    cout << "Debug: found : " << c << endl;

  }

}

```



And it runs as ...



```sh

$ time ./Test

Euler #49 ...

Found arithmetic progression: 1487 -> 4817 -> 8147

Found arithmetic progression: 1487 -> 4817 -> 8147

Found arithmetic progression: 2969 -> 6299 -> 9629

Found arithmetic progression: 2969 -> 6299 -> 9629

Debug: found : 1478

Debug: found : 1487

Debug: found : 2699

Debug: found : 2969



real    0m0.019s

user    0m0.016s

sys     0m0.000s

```
