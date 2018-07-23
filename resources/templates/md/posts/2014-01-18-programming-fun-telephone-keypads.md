{:title "programming fun telephone keypads"
:layout :post
 :tags ["old-post"]}



[This is a _medium_ level problem](http://www.reddit.com/r/dailyprogrammer/comments/1sody4/12113_challenge_139_intermediate_telephone_keypads/)  with two levels of difficulty.



```

Input Description

-----------------



On standard console input, you will be given an array of digits (0 to 9) and

spaces. All digits will be space-delimited, unless the digits represent

multiple presses of the same button (for example pressing 2 twice gives you the

letter 'B').  Use the modern Telephone Keypads digit-letter layout:

0 = Not used

1 = Not used

2 = ABC

3 = DEF

4 = GHI

5 = JKL

6 = MNO

7 = PQRS

8 = TUV

9 = WXYZ

You may use any source for looking up English-language words, though this

simple English-language dictionary is complete enough for the challenge.



Output Description

------------------

Print a list of all best-fitting words, meaning words that start with the word

generated using the given input on a telephone keypad. You do not have to only

print words of the same length as the input (e.g. even if the input is

4-digits, it's possible there are many long words that start with those

4-digits).

```



You could do this by reading the dictionary into a vector of strings, then figuring out the prefix determined by the input and matching it against each dictionary word. There is a better way to do this though, which makes more sense considering the second part of this question.



Instead of using a vector, you could use a [Trie](http://en.wikipedia.org/wiki/Trie) which in this case would look something like:



(showing the words 'sailor', 'soldier' and 'solder')



{% img center http://farm6.staticflickr.com/5534/12005243014_69145160ae_o_d.jpg 400 %}



This is a sort of naive implementation, you could compress this by storing strings and only expanding out to character chains if needed, but I skipped that optimization here.



{% gist 8422418 %}



There is a harder version of this problem:



```

Challenge++

-----------

If you want an extra challenge, accomplish the same challenge but without

knowing the number of times a digit is pressed. For example "7653" could mean

sold, or poke, or even solenoid! You must do this efficiently with regards to

Big-O complexity.

```



This would have been more inefficient with a vector of strings (you would sort the vector and then for each prefix, print out all subsequent ones until you encounter a string with a different prefix. So it's O(n log n) where n is the size of the dictionary).



With our trie, we can (once we have a list of all combinatorially possible prefixes) look them up _quickly_ (O(k + n), where k is the length of the prefix; O(k) time for each lookup and O(n) time for creating the trie (or O(mn) where m is the length of the largest word, though I would consider that a constant here)).



(Note: there's some duplication of common code, especially in the `Trie` class; sorry about that!)



{% gist 8460783 %}


