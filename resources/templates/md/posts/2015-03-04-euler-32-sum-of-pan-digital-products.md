{:title "euler 32 sum of pan digital products"
:layout :post
 :tags ["old-post"]}



Pretty dumb naive solution.



**Statutory Warning**: _Spoilers ahead_



```c++

using std::vector;

static const int kNumDigits = 9;



int getNumber(const vector<int>& vNum) {

  int result = 0;

  for (int i = 0; i < vNum.size(); ++i) {

    result = result * 10 + vNum[i];

  }

  return result;

}



bool isProduct(const vector<int>& vMultA,

               const vector<int>& vMultB,

               const vector<int>& vProduct) {

  int multa = getNumber(vMultA);

  int multb = getNumber(vMultB);

  int product = getNumber(vProduct);

  bool result = (multa * multb == product);

  if (result) {

    std::cout << "Debug: Testing " << multa << " * " << multb << " = " << product

              << "  --- MATCH!\n";

  }

  return result;

}



// Try all possible splits from this combination

void tryPermutation(const vector<int>& p,

                    int* total_matches,

                    std::unordered_set<int>* products) {

  assert(p.size() == kNumDigits);

  // There are nine digits, indexed from 0 to 8

  // First number spans 0 to i, the second i + 1 to j, third is from j + 1 to 8

  for (int i = 0; i <= kNumDigits - 3; ++i) {

    for (int j = i+1; j <= kNumDigits - 2; ++j) {

      vector<int> n1, n2, n3;

      for (int k = 0; k <= i; ++k) {

        n1.push_back(p[k]);

      }

      for (int k = i+1; k <= j; ++k) {

        n2.push_back(p[k]);

      }

      for (int k = j+1; k <= kNumDigits-1; ++k) {

        n3.push_back(p[k]);

      }

      if (isProduct(n1,n2,n3)) {

        ++(*total_matches);

        products->insert(getNumber(n3));

      }

    }

  }

}



int main() {

  vector<int> digits;

  for (int i = 1; i <= kNumDigits; ++i) {

    digits.push_back(i);

  }

  int total_matches = 0;

  std::unordered_set<int> products;

  do {

    tryPermutation(digits, &total_matches, &products);

  } while (std::next_permutation(digits.begin(), digits.end()));

  int products_sum = 0;

  for (const auto& p : products) {

    products_sum += p;

  }

  std::cout << "Number of matches = " << total_matches << ", "

            << "Sum of products = " << products_sum << std::endl;

}



```



Runs as follows:



```sh

~/cpp $ time ./Test

Debug: Testing 12 * 483 = 5796  --- MATCH!

Debug: Testing 138 * 42 = 5796  --- MATCH!

Debug: Testing 157 * 28 = 4396  --- MATCH!

Debug: Testing 159 * 48 = 7632  --- MATCH!

Debug: Testing 1738 * 4 = 6952  --- MATCH!

Debug: Testing 18 * 297 = 5346  --- MATCH!

Debug: Testing 186 * 39 = 7254  --- MATCH!

Debug: Testing 1963 * 4 = 7852  --- MATCH!

Debug: Testing 198 * 27 = 5346  --- MATCH!

Debug: Testing 27 * 198 = 5346  --- MATCH!

Debug: Testing 28 * 157 = 4396  --- MATCH!

Debug: Testing 297 * 18 = 5346  --- MATCH!

Debug: Testing 39 * 186 = 7254  --- MATCH!

Debug: Testing 4 * 1738 = 6952  --- MATCH!

Debug: Testing 4 * 1963 = 7852  --- MATCH!

Debug: Testing 42 * 138 = 5796  --- MATCH!

Debug: Testing 48 * 159 = 7632  --- MATCH!

Debug: Testing 483 * 12 = 5796  --- MATCH!

Number of matches = 18, Sum of products = 45228

15.517 secs

```



(Wait, why C++ instead of ... other recent languages? Couple of reasons: (1) I thought I'd need to brute force this (though it eventually turned out to take much less time than I anticipated), and (2) I'm sort of _over_ the _over-experimentation_ with languages that I'm not _really_ going to use, and I'm not going to get much out of anyway. Yes, tough. Deal with it)
