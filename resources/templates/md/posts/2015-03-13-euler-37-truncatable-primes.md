{:title "euler 37 truncatable primes"
:layout :post
 :tags ["old-post"]}



I realize using _C++_ is a bit like cheating since the initial motive of doing ProjectEuler was to explore a new _language_. But I reserve the right to "revise" that motive :P. Besides, I find it hard to overrule the part of myself that just wants to know the answer _now_, _quickly_.



**Statutory Warning**: _Spoilers ahead_



```c++

static const int kMaxNumbers = 1000000;



void filterPrimes(vector<bool>* numbers) {

  for (int candidate = 2; candidate < kMaxNumbers; ) {

    int multiple = candidate * 2;

    while (multiple < kMaxNumbers) {

      numbers->at(multiple) = false;

      multiple += candidate;

    }

    ++candidate;

    while (candidate < kMaxNumbers && !numbers->at(candidate)) {

      ++candidate;

    }

    if (candidate == kMaxNumbers) {

      return;

    }

  }

}



int getNumber(const vector<int>& digits, int start, int end) {

  int num = 0;

  for (int i = end; i >= start; --i) {

    num = num * 10 + digits[i];

  }

  return num;

}



bool isTruncatablePrime(int num, const vector<bool>& primes) {

  if (num == 2 || num == 3 || num == 5 || num == 7) {

    return false;

  }

  assert(primes.size() == kMaxNumbers);

  vector<int> digits;

  for (int t = num; t > 0; t /= 10) {

    digits.push_back(t % 10);

  }

  for (int i = 0; i < digits.size(); ++i) {

    int n1 = getNumber(digits, 0, i);

    int n2 = getNumber(digits, i, digits.size() - 1);

    if (!primes[n1] || !primes[n2]) {

      return false;

    }

  }

  return true;

}



int main() {

  vector<bool> prime_candidates(kMaxNumbers, true);

  prime_candidates[0] = false;

  prime_candidates[1] = false;

  filterPrimes(&prime_candidates);

  int sum_truncatables = 0;

  for (int i = 13; i < kMaxNumbers; ++i) {

    if (prime_candidates[i]) {

      if (isTruncatablePrime(i, prime_candidates)) {

        std::cout << "Debug: found " << i << std::endl;

		sum_truncatables += i;

      }

    }

  }

  std::cout << "The sum is: " << sum_truncatables << std::endl;

}

```



which runs as



```sh

Debug: found 23

Debug: found 37

Debug: found 53

Debug: found 73

Debug: found 313

Debug: found 317

Debug: found 373

Debug: found 797

Debug: found 3137

Debug: found 3797

Debug: found 739397

The sum is: <redacted>

```
