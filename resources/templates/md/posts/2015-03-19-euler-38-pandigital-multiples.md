{:title "euler 38 pandigital multiples"
:layout :post
 :tags ["old-post"]}



I'm embarassed at how ridiculously naive my solution is. In hindsight, it's so _obvious_ that the solution _has_ to be a four-digit number and its double, and that the first digit is going to be a 9.



Still, C++ makes you lazy. This took `24 milliseconds`, and I didn't feel the need to speed it up :)



**Statutory Warning**: _Spoilers ahead_



```c++

static const long kMaxNumbers = 10000;



int main() {

  long largestPanDigital = 0;

  for (long num = 2; num < kMaxNumbers; ++num) {

    vector<int> pandigits;

    int o = 1;

    while (o <= 20) {

      long product = num * o;

      vector<int> pd;

      while (product > 0) {

        int d = product % 10;

        if (d == 0 ||

            (std::find(pandigits.begin(), pandigits.end(), d) != pandigits.end()) ||

            (std::find(pd.begin(), pd.end(), d) != pd.end())) {

          break;

        }

        pd.push_back(d);

        product /= 10;

      }

      if (product > 0) {

        // Found an existing digit

        break;

      }

      // We formed a new product; keep going!

      for (auto it = pd.rbegin(); it != pd.rend(); ++it) {

        pandigits.push_back(*it);

      }

      ++o;

    }

    if (pandigits.size() == 9 && o > 1) {

      long panDigital = 0L;

      std::cout << "Found: ";

      for (auto& d : pandigits) {

        std::cout << d;

        panDigital = panDigital * 10 + d;

      }

      std::cout << " = " << num << " * " << o-1 << std::endl;

      if (panDigital > largestPanDigital) {

        largestPanDigital = panDigital;

      }

    }

  }

  std::cout << "The largest one is " << largestPanDigital << std::endl;

}

```



It runs as follows:



```sh

$ ~/cpp/Test

Found: 918273645 = 9 * 5

Found: 192384576 = 192 * 3

Found: 219438657 = 219 * 3

Found: 273546819 = 273 * 3

Found: 327654981 = 327 * 3

Found: 672913458 = 6729 * 2

Found: 679213584 = 6792 * 2

Found: 692713854 = 6927 * 2

Found: 726914538 = 7269 * 2

Found: 729314586 = 7293 * 2

Found: 732914658 = 7329 * 2

Found: 769215384 = 7692 * 2

Found: 792315846 = 7923 * 2

Found: 793215864 = 7932 * 2

Found: 926718534 = 9267 * 2

Found: 927318546 = 9273 * 2

Found: 932718654 = 9327 * 2

The largest one is <redacted>

```
