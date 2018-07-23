{:title "euler 44 triangle"
:layout :post
 :tags ["old-post"]}



While we're doing brute force solutions, here's another one ...



**Statutory Warning**: _Spoilers Ahead_



```c++

uint64_t get_triangle(uint64_t n) {

  return n * (n + 1) / 2;

}



bool is_pentagonal(uint64_t num) {

  // y = x * (3*x - 1) / 2

  // So x is an integral solution of 3x^2 + (-1)x + (-2y) = 0

  // ... or (1 + sqrt(1 + 24y))/6

  

  double solution = (1 + sqrt(1 + 24 * num)) / 6.0;

  double dummy;

  return (std::modf(solution, &dummy) == 0.0);

}



bool is_hexagonal(uint64_t num) {

  // y = x * (2*x - 1)

  // So x is an integral solution of 2x^2 + (-1)x + (-y) = 0

  // ... or (1 + sqrt(1 + 8y))/4



  double solution = (1 + sqrt(1 + 8 * num)) / 4.0;

  double dummy;

  return (std::modf(solution, &dummy) == 0.0);

}



void sanity_check() {

  uint64_t t285 = get_triangle(285);

  assert(t285 == 40755);

  assert(is_pentagonal(t285));

  assert(is_hexagonal(t285));

}



int main() {

  cout << "Euler # 45 ... \n\n";

  

  sanity_check();

  // We already now that T_285 is pentagonal and hexagonal, so start

  // from the next index;

  uint64_t i = 286;

  for (; ; ++i) {

    uint64_t num = get_triangle(i);

    if (is_pentagonal(num) && is_hexagonal(num)) {

      cout << "Found triangle# " << i << "  (" << num << ")" << endl;

      return 0;

    }

    if (i % 100000 == 0) {

      cout << "Done testing " << i << endl;

    }

  }

}

```
