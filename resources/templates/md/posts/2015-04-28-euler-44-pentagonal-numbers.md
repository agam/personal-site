{:title "euler 44 pentagonal numbers"
:layout :post
 :tags ["old-post"]}



I said I'd try to work out the next problem on paper, but I gave up and wrote a program for it again :(



I honestly thought it would take a long time to run, with the final answer some grotesquely long number ... but I was mistaken. ProjectEuler agrees with this assessment, since this happens to be officially categorized as among the _fifty easiest problems_ (!)



It took just `90ms` to run, which is simply too much of a tradeoff to pass up.



**Statutory Warning**: _Spoilers Ahead_



```c++

uint64_t get_pentagonal(int n) {

  assert(n > 0);

  return n * (3 * n - 1) / 2;

}



bool is_pentagonal(uint64_t num) {

  // y = x * (3*x - 1) / 2

  // So x is an integral solution of 3x^2 + (-1)x + (-2y) = 0

  // ... or (1 + sqrt(1 + 24y))/6

  

  double solution = (1 + sqrt(1 + 24 * num)) / 6.0;

  double dummy;



  // Check if the solution is an integer

  return (std::modf(solution, &dummy) == 0.0);

}



bool check_pentagonal(int i, int j) {

  assert(i > 0 && j > 0);

  assert(j < i);

  uint64_t penta_i = get_pentagonal(i);

  uint64_t penta_j = get_pentagonal(j);

  uint64_t sum = penta_i + penta_j;

  uint64_t diff = penta_i - penta_j;

  if (is_pentagonal(sum) && is_pentagonal(diff)) {

    cout << "Found match for i = " << i

         << " and j = " << j

         << ", with diff = " << diff << endl;

    return true;

  } else {

    return false;

  }

}



int main() {

  cout << "Euler # 44 ... \n\n";

  int i = 0;

  for (int i = 0; ; ++i) {

    for (int j = i-1; j > 0; --j) {

      if (check_pentagonal(i,j)) {

        return 0;

      }

    }

    if (i % 1000 == 0) {

      cout << "Done testing " << i << endl;

    }

  }

}

```
