{:title "syntax transformers"
:layout :post
 :tags ["old-post"]}



I've seen `defmacro` before, and while the pros and cons of "hygiene" are contentious, I had never bothered to learn macros in Scheme before. So now I'm working my way through Kent Dybvig's ["The Scheme Programming Language"](http://www.scheme.com/tspl4/), and I have to say, I like what I see. Here's an example I wrote today:



```scheme

;;; Exercise 3.1.3

(define-syntax my-let*

  (syntax-rules ()

    [(_ ((x e)) body ...)

      (let ((x e))

        body ...)]

    [(_ ((x1 e1) (x2 e2) ...) body ...)

      (let ((x1 e1))

        (my-let* ((x2 e2) ...) body ...))]))

```



Used in the REPL as follows:



```scheme

> (my-let* ([a 5] [b (+ a a)] [c (+ a b)])

    (list a b c))

'(5 10 15)

```
