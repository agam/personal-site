{:title "euler 34 curious numbers"
:layout :post
 :tags ["old-post"]}



_Grumble_ ... I'm missing the tougher problems we saw earlier. This is another easy one.



**Statutory Warning**: _Spoilers ahead_



```c++

using std::vector;



vector<int> getDigits(int number) {

  vector<int> v;

  while (number > 0) {

    v.push_back(number % 10);

    number /= 10;

  }

  return v;

}



void printVector(const vector<int>& v) {

  std::cout << "[ ";

  for (const auto& d : v) {

    std::cout << d << ", ";

  }

  std::cout << " ]\n";

}



int factorial(int n) {

  // Yep, crude, but it works.

  static std::unordered_map<int,int> memos;

  const auto& it = memos.find(n);

  if (it != memos.end()) {

    return it->second;

  }

  int fact = 1;

  for (int i = 1; i <= n; ++i) {

    fact *= i;

  }

  memos[n] = fact;

  return fact;

}



int sumFactDigits(const vector<int>& v) {

  int sum = 0;

  for (const auto& d : v) {

    sum += factorial(d);

  }

  return sum;

}



int main() {

  static const int kMaxTries = 1000000;

  long sum = 0;

  for (int i = 3; i < kMaxTries; ++i) {

    vector<int> d = getDigits(i);

    if (i == sumFactDigits(d)) {

      sum += i;

      std::cout << "Debug: found ";

	  std::reverse(d.begin(), d.end());

      printVector(d);

    }

  }

  std::cout << "Required sum = " << sum << std::endl;

}

```



... and it runs as



```sh

Debug: found [ 1, 4, 5,  ]

Debug: found [ 4, 0, 5, 8, 5,  ]

Required sum = <redacted>

```


