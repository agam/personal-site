{:title "euler 35 circular primes"
:layout :post
 :tags ["old-post"]}



The easy solutions continue (or, I've just gotten used to C++ for these)



**Statutory Warning**: _Spoilers ahead_



```c++

// Euler 35: A number is a circular prime if all the _rotations_ of

// its digits are prime numbers.



using std::vector;



const int kMaxNumbers = 1000000;

typedef std::bitset<kMaxNumbers> AllNumbers;



void markMultiples(AllNumbers* v, int n) {

  const int kStart = 2 * n;

  if (kStart >= kMaxNumbers) {

    return;

  }

  for (int i = kStart; i < kMaxNumbers; i += n) {

    v->set(i, false);

  }

}



void markPrimes(AllNumbers* v) {

  v->set(0, false);

  v->set(1, false);

  int i = 2;

  while (true) {

    markMultiples(v, i);

    ++i;

    for (; i < kMaxNumbers && !v->test(i); ++i) {}

    if (i == kMaxNumbers) {

      return;

    }

  }

}



vector<int> getDigits(int n) {

  vector<int> v;

  while (n > 0) {

    v.push_back(n % 10);

    n /= 10;

  }

  return v;

}



int getNumber(const vector<int>& v) {

  int n = 0;

  for (int i = v.size()-1; i >=0; --i) {

    n = n*10 + v[i];

  }

  return n;

}



vector<int> getRotatedNumbers(int n) {

  auto v = getDigits(n);

  vector<int> numbers;

  for (int i = 0; i < v.size(); ++i) {

    numbers.push_back(getNumber(v));

    std::rotate(v.begin(), v.end()-1, v.end());

  }

  return numbers;

}



int main() {

  AllNumbers v;

  for (int i = 0; i < kMaxNumbers; ++i) {

    v.set(i, true);

  }

  markPrimes(&v);

  int numCircularPrimes = 0;

  for (int i = 0; i < kMaxNumbers; ++i) {

    // Fail early

    if (!v.test(i)) {

      continue;

    }

    vector<int> ps = getRotatedNumbers(i);

    if (std::all_of(ps.begin(), ps.end(), [&v](int p){return v.test(p);})) {

      ++numCircularPrimes;

    }

  }

  std::cout << "Number of circular primes = " << numCircularPrimes << std::endl;

}

```



It runs in `335 milliseconds` -- though I'm not sure if it is a high or a low number.
