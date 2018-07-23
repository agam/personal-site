{:title "euler 36 palindromic numbers"
:layout :post
 :tags ["old-post"]}



Wha ... this is getting boring.



**Statutory Warning**: _Spoilers ahead_



```c++

using std::vector;

static const int kMaxNumbers = 1000000;



bool isPalindromic(int n, int base) {

  vector<int> digits;

  while (n > 0) {

    digits.push_back(n % base);

    n /= base;

  }

  int length = digits.size();

  for (int i = 0; i < length/2; ++i) {

    if (digits[i] != digits[length-1-i]) {

      return false;

    }

  }

  return true;

}



int main() {

  int sum = 0;

  for (int i = 1; i < kMaxNumbers; ++i) {

    if (isPalindromic(i, 10) && isPalindromic(i, 2)) {

      sum += i;

    }

  }

  std::cout << "Sum of palindromic numbers = " << sum << std::endl;

}

```



(runs in `931 milliseconds`)
