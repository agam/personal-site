{:title "euler 33 curious fractions"
:layout :post
 :tags ["old-post"]}



Hmm .. this is almost too straightforward to post, but here goes ...



**Statutory Warning**: _Spoilers ahead_



```c++

using std::pair;



pair<int,int> getDigits(int num) {

  return std::make_pair(num / 10, num % 10);

}



int gcd(int a, int b) {

  assert(b <= a);

  if (b == 0) {

    return a;

  } else {

    return gcd(b, a % b);

  }

}



pair<int,int> getReducedForm(int num, int den) {

  int g = gcd(den, num);

  return std::make_pair(num / g, den / g);

}



bool isCuriousFraction(int num, int den) {

  // If (1) the numbers share digits, and (2) they can be reduced,

  // then they form a 'curious fraction'

  auto reducedForm = getReducedForm(num, den);



  auto n = getDigits(num);

  auto d = getDigits(den);



  if ((n.second == 0 && d.second == 0) || n.first > d.second) {

    return false;

  }



  auto curiousForm = getReducedForm(n.first, d.second);



  if (n.second == d.first && curiousForm == reducedForm) {

    return true;

  } else {

    return false;

  }

}



int main() {

  int numProduct = 1, denProduct = 1;

  // Go through all pairs of two-digit numbers, where one is smaller than the other.

  for (int den = 10; den < 100; ++den) {

    for (int num = 10; num < den; ++num) {

      if (isCuriousFraction(num, den)) {

	std::cout << "Debug: found " << num << " / " << den << std::endl;

	numProduct *= num;

	denProduct *= den;

      }

    }

  }

  // For lowest common terms, divide both by GCD

  std::cout << "Denominator of product = "

            << denProduct / gcd(denProduct, numProduct) << std::endl;

}

```



... and runs as follows



```sh

Debug: found 16 / 64

Debug: found 26 / 65

Debug: found 19 / 95

Debug: found 49 / 98

Denominator of product = <redacted>

```



(_Sigh_ ... yes, its _longer_ than some of the previous solutions, but it really didn't take me long to write it ... I think I'm developing a renewed appreciation for C++)
