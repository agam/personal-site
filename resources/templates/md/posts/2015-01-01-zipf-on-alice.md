{:title "zipf on alice"
:layout :post
 :tags ["old-post"]}



I just encountered _Zipf's Law_[^1] for the first time, and wanted to confirm it for myself. So I got a copy of Alice in Wonderland[^2], removed the metadata header, and ran it through this quick Go program. 



```go

package main



import "fmt"

import "os"

import "bufio"

import "strings"

import "unicode"

import "sort"



type wordFrequencyMap map[string]int



func updateFrequencies(line string, wm wordFrequencyMap) {

	// Skip chapter titles

	if strings.Contains(line, "CHAPTER") {

		return

	}

	scanner := bufio.NewScanner(strings.NewReader(line))

	scanner.Split(bufio.ScanWords)

	for scanner.Scan() {

		// Might need to break word down further if punctuation exists

		word := strings.ToLower(scanner.Text())

		nextWord := ""

		for _, rune := range word {

			if !unicode.IsLetter(rune) {

				if len(nextWord) > 0 {

					wm[nextWord]++

				}

				nextWord = ""

			} else {

				nextWord += string(rune)

			}

		}

		if len(nextWord) > 0 {

			wm[nextWord]++

		}

	}

}



// Boilerplate to get sorting of keys by value to work

type Pair struct {

	k string

	v int

}



type PairList []Pair



func (p PairList) Swap(i, j int)      { p[i], p[j] = p[j], p[i] }

func (p PairList) Len() int           { return len(p) }

func (p PairList) Less(i, j int) bool { return p[i].v < p[j].v }



func showStats(wm wordFrequencyMap) {

	fmt.Println("Statistics on the text\n")

	fmt.Println("Number of words found: ", len(wm))



	numWordsTotal := 0

	numTopWords := 0

	numBotWords := 0

	freqProdTop := make([]int, 100)

	freqProdBot := make([]int, 100)

	pl := make(PairList, len(wm))

	i := 0

	for k, v := range wm {

		pl[i] = Pair{k, v}

		i++

		numWordsTotal += v

	}

	sort.Sort(sort.Reverse(pl))



	i = 0

	fmt.Println("\nThe top 100 words are :- ")

	for _, p := range pl {

		if i >= 100 {

			break

		}

		fmt.Printf("%30s -> %6d\n", p.k, p.v)

		freqProdTop[i] = p.v * (i + 1)

		i++

		numTopWords += p.v

	}



	sort.Sort(pl)

	i = 0

	fmt.Println("\nThe bottom 100 words are :-")

	for _, p := range pl {

		if i >= 100 {

			break

		}

		fmt.Printf("%30s -> %6d\n", p.k, p.v)

		freqProdBot[i] = p.v * (len(wm) - i)

		i++

		numBotWords += p.v

	}



	fmt.Println("\n\nFrequency products of top words :- ")

	for _, elem := range freqProdTop {

		fmt.Printf("%4d ", elem)

	}

	fmt.Println("\n\nFrequency product of bottom words :-")

	for _, elem := range freqProdBot {

		fmt.Printf("%4d ", elem)

	}

	fmt.Println("\n\nTotal occurrences = ", numWordsTotal)

	fmt.Println("Top words accounted for ", numTopWords)

	fmt.Println("Bottom words accounted for ", numBotWords)

}



func main() {

	wm := make(wordFrequencyMap)

	scanner := bufio.NewScanner(os.Stdin)

	for scanner.Scan() {

		line := scanner.Text()

		updateFrequencies(line, wm)

	}

	showStats(wm)

}

```



Yes, it's not written _super-efficiently_, but it runs _in a third of a second_, so I don't care; it's verbose but took less than five minutes to write, and is fairly straightforward.



Anyway, it runs as follows:



```sh

$ cat ~/Downloads/alice-in-wonderland.txt | go run word-frequency.go > /tmp/log

```



with output that looks like:



```

Statistics on the text



Number of words found:  1793



The top 100 words are :- 

                           the ->    667

                           and ->    467

                            to ->    415

                             a ->    351

                           she ->    350

                            it ->    343

                             i ->    331

                            of ->    247

                           was ->    214

                            in ->    188

                         alice ->    187

...

                         mouse ->     42

...

                        rabbit ->     26

...

                   caterpillar ->     26

...

<omitted selectively>



The bottom 100 words are :-

                          dish ->      1

                      inclined ->      1

                          beak ->      1

                      wandered ->      1

                        master ->      1

                        seldom ->      1

                     naturedly ->      1

...

<rest omitted>



Frequency products of top words :- 

 667  934 1245 1404 1750 2058 2317 1976 1926 1880 2057 2232 2171 2254 2205 2128 1632 1728 1824 1900 1953 2002 1932 1992 1975 2054 2133 2184 2204 2250 2263 2304 2211 2142 2100 2088 2146 2166 2223 2160 2214 2226 2236 2288 2295 2300 2256 2208 2254 2250 2295 2288 2226 2268 2310 2352 2337 2320 2301 2340 2318 2294 2331 2368 2340 2376 2278 2312 2346 2380 2414 2304 2336 2368 2325 2280 2310 2262 2291 2320 2268 2296 2324 2268 2295 2322 2349 2288 2314 2340 2366 2392 2418 2350 2375 2304 2328 2352 2376 2400 



Frequency product of bottom words :-

1793 1792 1791 1790 1789 1788 1787 1786 1785 1784 1783 1782 1781 1780 1779 1778 1777 1776 1775 1774 1773 1772 1771 1770 1769 1768 1767 1766 1765 1764 1763 1762 1761 1760 1759 1758 1757 1756 1755 1754 1753 1752 1751 1750 1749 1748 1747 1746 1745 1744 1743 1742 1741 1740 1739 1738 1737 1736 1735 1734 1733 1732 1731 1730 1729 1728 1727 1726 1725 1724 1723 1722 1721 1720 1719 1718 1717 1716 1715 1714 1713 1712 1711 1710 1709 1708 1707 1706 1705 1704 1703 1702 1701 1700 1699 1698 1697 1696 1695 1694 



Total occurrences =  13812

Top words accounted for  8469

Bottom words accounted for  100

```



Except for the first four words (_the_, _and_, _to_, _a_), the rest do appear to bear a product in (roughly) the same range, i.e. the frequency of a word is more or less inversely proportional to its rank.



[^1]: As in [this Wikipedia article](http://en.wikipedia.org/wiki/Zipf%27s_law), though he meant it [more generally](http://www.amazon.com/Human-Behavior-Principle-Least-Effort/dp/161427312X) than _merely_ linguistics.



[^2]: From [Gutenberg](http://www.gutenberg.org/ebooks/11)
