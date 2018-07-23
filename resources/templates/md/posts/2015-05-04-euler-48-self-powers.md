{:title "euler 48 self powers"
:layout :post
 :tags ["old-post"]}



One of the simplest problems, that would be a one-liner in Python or Lisp. Decided to do it in C++ anyway, using the `GMP` library (verbose, but runs in `4 ms`)



For this to work, you'll need to add (e.g. for `CMake`) a line like `target_link_libraries(Test gmpxx gmp)`, and include `gmpxx.h`.



**Statutory Warning**: _Spoilers Ahead_



```cpp

int main() {

  cout << "Euler #48 ... \n";

  const int kNumTerms = 1000;

  

  // GMP has a C++ interface, see the following link:

  // https://gmplib.org/manual/C_002b_002b-Interface-General.html

  mpz_class sum = 0;



  // Loop from 1 to 1000

  for (int i = 1; i <= kNumTerms; ++i) {

    mpz_class term = 0;

    

    // GMP exponentiation function, see documentation at:

    // https://gmplib.org/manual/Integer-Exponentiation.html

    mpz_ui_pow_ui(term.get_mpz_t(), i, i);

    

    sum = sum + term;

  }

  

  mpz_class last_ten_digits;



  // We're looking for sum modulo (10 ^ 10)

  mpz_class divisor;

  mpz_ui_pow_ui(divisor.get_mpz_t(), 10, 10);

  mpz_mod(last_ten_digits.get_mpz_t(), sum.get_mpz_t(), divisor.get_mpz_t());

  cout << "The final answer is : " << last_ten_digits << endl;

}

```
