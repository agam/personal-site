{:title "object thinking"
:layout :post
 :tags ["old-post"]}



I picked up [_"Object Thinking"_](http://www.amazon.com/Object-Thinking-Developer-Reference-David/dp/0735619654) by David West recently, which is an unusual computer programming text, in that it devotes much (most?) of its pages to "philosophy".



Somewhere towards the beginning, there's this example of writing a program to count the frequencies of words, first in Pascal, then in Smalltalk written like Pascal, and finally in Smalltalk written in the "proper" OO style.



I thought I'd attempt the same but with Go and Dart instead ...



**Note (1)**: this is _not_ about one language being better than another or something of the sort; the point is rather that the style is independent of the language, is meant only to contrast (in the author's opinion) purely procedural vs purely object-oriented styles of programming.



**Note (2)**: some of the code below may be idiomatic, don't get upset, this is just for fun, I'm trying to match the original Pascal and Smalltalk as closely as possible.



First of all, the classic procedural program.



```go

package main



import "fmt"

import "bufio"

import "os"



func main() {

	fmt.Printf("Enter line: ")

	bio := bufio.NewReader(os.Stdin)

	line, _, err := bio.ReadLine()

	if err != nil {

		return

	}

	var f [26]int

	for i := 0; i < 26; i++ {

		f[i] = 0

	}



	for i := 0; i < len(line); i++ {

		c := line[i]

		k := c - 'a'

		if k >= 0 && k < 26 {

			f[k] = f[k] + 1

		}

	}



	fmt.Println("Frequencies :-")

	for i := 0; i < 26; i++ {

		fmt.Printf("%d ", f[i])

	}

}

```



Second, a language that supports object-orientation, but used in a procedural style.



```dart

import 'dart:io';



main() {

  var f = new List(26);

  stdout.write("Enter line: ");

  var line = stdin.readLineSync();

        

  for (int i = 0; i < 26; i++) {

    f[i] = 0;

  }



  var lowerCaseA = "a".codeUnitAt(0);

  for (int i = 0; i < line.length; i++) {

    var c = line.codeUnitAt(i);

    var k = c - lowerCaseA;

    if (k >= 0 && k < 26) {

      f[k] = f[k] + 1;

    }

  }



  print("Frequencies :-");

  stdout.write(f);

}

```



Third, an object-oriented program (or, you could call it a functional program too, and then make a meta-point that I won't).



```dart

import 'dart:io';



main() {

  var f = new Map();

  stdout.write("Enter line: ");

  var line = stdin.readLineSync();

  line.runes.toList().forEach((e) {

        f.putIfAbsent(e, () => 0);

        f[e]++;

      });



  print("Frequencies :-");

  print(f);

}

```



Unfortunately, the original analogy might not be preserved very well, because (1) Dart is _way_ more verbose than Smalltalk, and (2) I showed whole programs here, rather than relevant fragments.
