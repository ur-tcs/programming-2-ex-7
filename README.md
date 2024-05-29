# Programming 2 - Exercise 7 : `for` comprehensions

Exercises marked with ⭐️ are the most important ones. Exercises marked with 🔥 are the most challenging ones. You do not need to complete all exercises to succeed in this class, and you do not need to do all exercises in the order they are written.

Scala does not have `for` loops; instead, it has `for` comprehensions. Comprehensions are particularly powerful when you need to combine and filter results from multiple collections. Let’s see some examples!

## Warm-up ⭐️

This week, we have a new way to to filter, map over, and flatten lists: comprehensions.

### *Just 3 (filters)*

Using a `for` comprehension, write a function that filters a list of words (represented as `String`s), keeping only words of length 3:

```scala
def onlyThreeLetterWords(words: List[String]): List[String] =
    ???
```
(src/main/scala/comprehensions/forcomp.scala)

### *LOUDER (maps)*

Using a `for` comprehension, write a function that converts a list of words (represented as `String`s) to uppercase, using the `.toUpperCase()` method:

```scala
def louder(words: List[String]): List[String] =
  ???
```
(src/main/scala/comprehensions/forcomp.scala)

For example `List("a", "bc", "def", "ghij")` should become `List("A", "BC", "DEF", "GHIJ")`.

> **WARNING:**
>
>When using internationalization-related methods, such as `.toUpperCase(`*locale*`)` or `.toLowerCase(`*locale*`)`, always be careful about which [*locale*](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) parameter you use.
>
>If omitted, locale defaults to the user’s current locale. On some computers, this will create issues with our tests: for example, on a a Turkish system, calling `"TITLE".toLowerCase()` will yield `"tıtle"`, not `"title"` (you can reproduce this behavior by passing `locale = Locale("tr")`.
>
>Locale-related bugs are a common source of issues in real-world apps — think hard about the right value, and if writing locale-independent code, use the standardized invariant locale [Locale.ROOT](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#ROOT).

### *Echo (flatMaps)*

Using a `for` comprehension, write a function that repeats each word in a list of words `n` times. Write a comprehension with two separate `<-` clauses, and use either `Iterable.fill(n)(word)` to create an iterable of `n` times the value `word`, or `(1 to n)` to iterate `n` times.

```scala
def echo(words: List[String], n: Int): List[String] =
  ???
```
(src/main/scala/comprehensions/forcomp.scala)

For example `List("a", "bc", "def", "ghij")` should become `List("a", "a", "bc", "bc", "def", "def", "ghij", "ghij")` if `n == 2`.

What should happen if `n == 1`? How about `n == 0`?

<details>
    <summary>SOLUTION</summary>
    For `n == 1`, the resulting list is the same as the source string. For `n == 0`, the resulting list is empty.
</details><br/>

### *All together now*

Using a `for` comprehension, write a function that converts all words in a list to upper case, removes all words whose length is not three, and repeats all others `n` times:

```scala
def allTogether(words: List[String], n: Int): List[String] =
  ???
```
(src/main/scala/comprehensions/forcomp.scala)

For example `List("All", "together", "now")` should become `List("ALL", "ALL", "ALL", "NOW", "NOW", "NOW")` if `n is 3`.

## Cross product

Reimplement the `cross` function from last week using a `for` comprehension.

Additionally, your function should now take a Scala `List` as input, and return a Scala `List` as output.

```scala
def cross[A, B](l1: List[A], l2: List[B]): List[(A, B)] =
  ???
```
(src/main/scala/comprehensions/forcomp.scala)

## Triangles in a directed graph

Reimplement the `triangles` function from last week using a `for` comprehension and Scala `Lists`.

```scala
def triangles(edges: DirectedGraph): List[(NodeId, NodeId, NodeId)] =
  ???
```
(src/main/scala/comprehensions/forcomp.scala)

## Glob matching (ungraded callback to `find`!)

The real Unix `find` command takes a “glob pattern” for its `-name` filter: it supports *wildcards* like `?` and `*` to allow for partial matches. For example,

- find `-name 2023-*.jpg` finds all files whose name starts with `2023-` and ends with `.jpg`, and
- find `-name 20??-*.jp*g` finds all files whose name starts with `20`, then has two arbitrary letters, then a dash, any letter, and finally `.jp` followed by anything followed by `g`. This would allow users to find `2002-03-18T11:15.jpg` or `2002-03-18 modified.jpeg`, for example.

Write a function `glob(pattern, input)` that returns a boolean indicating whether a glob pattern matches a string (represented as a list of `Char`s):

```scala
def glob(pattern: List[Char], input: List[Char]): Boolean =
  ???
```
(src/main/scala/comprehensions/Glob.scala)

The rules are as follows:

- `?` matches one arbitrary character
- `*` matches an arbitrary sequence of characters
- Other characters match themselves

The whole pattern must match the whole string: partial matches are not allowed.

<details>
    <summary>IMPLEMENTATION GUIDE</summary>
    This is a tricky problem at first sight, but it admits a very nice recursive solution. Think of it in groups: how can you reduce the problem of a matching a string against a pattern to a problem with a smaller string, or a smaller pattern?
</details>

Based on your implementation, can you prove that a pattern that does not contain wildcards matches only itself? That “*” matches everything? That a pattern with only `?` matches all strings of the same length as the pattern? That repeated `*` can be replaced by single `*`?

> **NOTE:**
>
>If you plug in your new function into your copy of find, you’ll get an even better file-searcher! It should be a very straightforward refactoring — just change the function passed to the higher-order find function that we wrote in the last callback.

## Des chiffres et des lettres

*“Des chiffres et des lettres”* is a popular TV show in French-speaking countries.

In this show, contestants take turns guessing the longest word that can be made from a list of letters, and finding a way to arrange a list of numbers into an arithmetic computation to get as close as possible to a target number. For example:

- For the letters `G N Q E U T I O L Y C E P H`, an excellent contestant would immediately propose the word “POLYTECHNIQUE”.

- For the numbers `2 5 3 9 100 9` and the target number `304`, an excellent contestant would suggest `3 * (9 + 9) + 5 * 100 / 2`, and exclaim “le compte est bon!”.

### Des lettres

Write two functions `longestWord` which, given a wordlist (a collection of words) represented as a `List[String]` and a collection of letters represented as a `String`, finds the longest word that can be made by reordering a subset of these strings. You can write your function directly, or follow the steps below.

**STEP-BY-STEP-HINTS**

Let’s represent collections of letters by converting them to uppercase an sorting them, so that `"Polytechnique"` becomes `"CEEHILNOPQTUY"`. We call `"Polytechnique"` the “original” word, and `"CEEHILNOPQTUY"` the “scrambled” word.

1. Write a function scramble that transforms a single word into its scrambled representation.

    ```scala
    def scramble(word: String): String =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

    <details>
        <summary>HINT</summary>
        You may find `.toUpperCase()` and `.sorted` useful.
    </details></br>
 
2. Write a function scrambleList that transforms a wordlist into a Map from scrambled words to sets of original words.

    For example, Set("Nale", "lean", "wasp", "swap") should become Map("AELN" -> Set("Nale", "lean"), "APSW" -> Set("wasp", "swap"))

    ```scala
    def scrambleList(allWords: Set[String]): Map[String, Set[String]] =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

    <details>
        <summary>HINT</summary>
        Consider using `.groupBy` to create the map.
    </details></br>

3. Write a function exactWord that returns all words of a wordlist that can be formed by using all letters from a given collection of letters.

    ```scala
    def exactWord(allWords: Set[String], letters: String): Set[String] =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

4. Write a function compatible that checks whether a scrambled word can be formed from a collection of letters. Beware of repeated letters!

    ```scala
    def compatible(small: String, large: String): Boolean =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

5. Write a function longestWord that returns the longest word that can be formed using some letters from a given collection of letters.

    ```scala
    def longestWord(allWords: Set[String], letters: String): Set[String] =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

### Des chiffres 🔥

[Example video](https://www.youtube.com/watch?v=PRHj78ct_JY) 

Write a function `leCompteEstBon` that takes a `List` of integers and a target number, and returns an arithmetic expression that evaluates to the target sum, if one exists. The allowable operators are `+`, `-` (only when the result is nonnegative), `*`, and `/` (only when the division is exact), as well as parentheses. For the purpose of this exercise, assume that we are only interested in exact results, using all provided integers. The result should be an expression of type `Expr`:

**THE `EXPR` TYPE**

We have provided you with an `Expr` type as a starting point:

```scala
trait Expr:
    val value: Option[Int]
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala

Notice that each `Expr` reports its own value as an `Option[Int]`: this is because
this type has two direct subclasses: numbers and binary operators:

```scala
case class Num(n: Int) extends Expr:
  val value = Some(n)
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

```scala
abstract class Binop extends Expr:
  val e1, e2: Expr // Subexpressions
  def op(n1: Int, n2: Int): Option[Int] // How to evaluate this operator
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

The `value` of a binop is defined thus, using `for` to unpack options:

```scala
val value: Option[Int] =
  for
    n1 <- e1.value
    n2 <- e2.value
    r <- op(n1, n2)
  yield r
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

Finally, the four arithmetic operators are subclasses of `Binop`:

```scala
case class Add(e1: Expr, e2: Expr) extends Binop:
  def op(n1: Int, n2: Int) =
    Some(n1 + n2)
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

```scala
case class Sub(e1: Expr, e2: Expr) extends Binop:
  def op(n1: Int, n2: Int) =
    if n1 < n2 then None else Some(n1 - n2)
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

```scala
case class Mul(e1: Expr, e2: Expr) extends Binop:
  def op(n1: Int, n2: Int) =
    Some(n1 * n2)
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

```scala
case class Div(e1: Expr, e2: Expr) extends Binop:
  def op(n1: Int, n2: Int) =
    if n2 != 0 && n1 % n2 == 0 then Some(n1 / n2) else None
```
(src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

Note how `-` and `/` sometimes return `None`.

> **HINT**
>
>Our solution is short, and uses a combination of `for` comprehensions and recursion. Take the time to think through how you might divide this problem into smaller subproblems.
>
>There are broadly two possible approaches: a top-down one, which splits the input set into two halves, and combines them with an operator; or a bottom-up 
one, which combines numbers into increasingly larger expression trees.

**STEP-BY-STEP HINTS (TOP-DOWN)**

1. Write a recursive function partitions which generates all partitions of a list into two non-overlapping sublists.

    ```scala
    def partitions[A](l: List[A]): List[(List[A], List[A])] =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

    <details>
        <summary>HINT</summary>
        This function must decide, for each element, whether it goes into the left or the right partition.
    </details>

2. Write a recursive function allTrees that generates all possible trees of expressions from a set of numbers using partitions.

    ```scala
    def allTrees(ints: List[Int]): List[Expr] =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

    <details>
        <summary>HINT</summary>
        At each step, this function should call itself recursively twice, once per subset, and generate one tree per operator.
    </details>

3. Write a recursive function leCompteEstBon that finds an expression among the possible ones that match the target number, or returns None if the target cannot be achieved:

    ```scala
    def leCompteEstBon(ints: List[Int], target: Int): Option[Expr] =
        ???
    ```
    (src/main/scala/comprehensions/DesChiffresEtDesLettres.scala)

    >**HINT:**
    >
    >Since the steps above are only suggestions, we have provided only integration tests (tests for `leCompteEstBon`), and no unit tests for intermediate functions. Make sure to write a few unit tests to make sure you understand what each function does before starting! (And feel free to share them with other students on Ed!)
    
</br>

>**NOTE:**
>
>You may find that your function takes a long time to return on our tests. In that case, write your own small tests to make sure that it works, then study it by running it on examples to understand where it wastes time. Can you think of optimizations?
>
><details>
>   <summary>HINT</summary>
>   Focus on `allTrees`. Does it really have to return *all* trees? For example, given the set `List(2, 2)`, is it valuable to return both `Add(Num(2)`, `Num(2))` and `Mul(Num(2)`, `Num(2))`? Similarly, on the set `List(2, 3, 4)`, is it valuable to keep both `2 * 3 + 4` and `3 * 4 - 2`? Our solution keeps just one of each, and this speeds it up from multiple seconds per problem to just a few milliseconds.
></details>
></br>

## Anagrams

What do the words *Suisse* and *Issues* have in common? They are *anagrams* of each other! Two words are called anagrams if they have the same letters, albeit in a different order. Sentences can also be anagrams of each other:

Finding anagrams is a tricky combinatorial problem: the letters in a sentence like [I AM LORD VOLDEMORT](https://www.youtube.com/watch?v=EEFsJ-43pnA), with 19 characters, can be reordered in 19!
19! ways, which is…

```scala
scala> def fact(n: BigInt): BigInt =
     |   if n == 1 then 1 else n * fact(n-1)
     | fact(19)
def fact(n: BigInt): BigInt
val res0: BigInt = 121645100408832000
```

… a lot. In this lab, we’ll create an anagram solver: a program capable of searching through a large space of potential anagrams more efficiently than by brute-force enumeration.


> **NOTE:**
>
>This lab aims to exercise the following concepts and techniques:
>
>- Types and type aliases
>- Non-structural recursion
>- Collections and their APIs
>- Comprehensions and combinatorial search
>
></br>

### Logistics

The assignment is built so that almost all of it can be solved by combining built-in functions and for comprehensions. Hence, if you find yourself writing a complicated recursive function, think again!

### Problem statement

An anagram of a word is another word that can be formed by rearranging the letters of the first. For example, `integral` is an anagram of `triangle`, and `logarithm` is an anagram of `algorithm`.

In a similar way, an anagram of a sentence is a different sentence that can be formed by rearranging the letters of the first. The new sentence must consist of meaningful words, but the number of words, punctuation, case, or even accents do not have to match. For example, `You olive` is an anagram of `I love you`. Plain permutations, such as `you I love`, are also anagrams for our purposes.

Your ultimate goal is to implement a method `sentenceAnagrams`, which, given a sentence (a list of words) and a list of permissible words, finds all the anagrams of that sentence composed of valid words.

Check yourself!

Take a moment to think about this problem. If you have not completed it yet, also consider taking a moment to solve the other exercises, especially the Des lettres part of Des chiffres et des lettres.

>**NOTE:**
>
>The approach in the first part of this lab is relatively inefficient; to keep your workload reasonable, we’ve left optimizations to an optional section at the end.

### Approach

To find sentence anagrams, we will break our sentence into individual letters, enumerate all subsets of these letters, check each subset to see if it is an anagram of a known word, and if so recursively combine it with sentence anagrams of the rest of the sentence.

For example, the sentence `You olive` has the letters `eiloouvy`:

1. Start by identifying a subset, say `elov`. We are left with the characters `iouy`.
2. Consult the word list to see that `elov` is an anagram of `love`.
3. Solve the problem recursively for the rest of the characters (`iouy`), obtaining `List(I, you)`, and `List(you, I)`.
4. Combine the word love to obtain six possible results:
    - `List(love, I, you)`,
    - `List(love, you, I)`,
    - `List(I, love, you)`,
    - `List(you, love, I)`,
    - `List(I, you, love)`, and
    - `List(you, I, love)`.

This process requires:

1. Normalizing all inputs to remove diacritics (accents) and non-alphabetic characters (punctuation, spaces, etc.)
2. Constructing a dictionary to quickly look up word anagrams
3. Recursively enumerating subsets of a given set
4. Recursively constructing sentence anagrams

… which is what we’ll do!

### Representation

We represent words of a sentence with the `String` data type, and sentences as lists of `Word`s:

```scala
type Word = String
type Sentence = List[Word]
```

### Finding single-word anagrams

To find anagrams of a single word, we could simply go through the whole word list and check for each entry whether it is an anagram of our input word. Unfortunately, this is not very efficient: we would need to scan the whole word list every time.

Instead, we will use a simple and very useful trick called *canonicalization*. Canonicalization is the process by which we translate multiple equivalent inputs (such as the words “eat” and “tea”) to a unique *canonical* representation, which we can then use to check equivalence quickly.

In *Des lettres*, we canonicalized words by “scrambling” them: converting them to uppercase and sorting them. `scramble` was such that two words w1 and w2 were anagrams if and only `if scramble(`w1`) == scramble(`w2`):`

```scala
scala> println(scramble("eat"))
AET
scala> println(scramble("tea"))
AET
```

The key observation here is that two words are anagrams if the multisets of their letters are equal. In this lab, we will use a different canonical representation of multisets, called *occurrence lists*: each word will be transformed into a unique list indicating how often each character appears (not unlike the lists in the Huffman lab that counted words). For example:

```scala
scala> import anagrams.Anagrams.*

scala> println(computeOccurrenceList("tea"))
List((a,1), (e,1), (t,1))

scala> println(computeOccurrenceList("eat"))
List((a,1), (e,1), (t,1))
```

To ensure uniqueness, we enforce that occurrence lists are sorted alphabetically and contain only lowercase characters. We represent them as Scala lists of pairs:

```scala
type OccurrenceList = List[(Char, Int)]
```

The integer in each pair denotes how many times a character appears in a particular word or a sentence. This integer must be positive – characters that do not appear in the sentence do not appear in the occurrence list either.

### Computing Occurrence Lists

1. Implement the function `computeOccurrenceList` that takes a string, *normalizes* it, and returns a the corresponding occurrence list. For your convenience, we have provided a function `normalizeString` that removes removes diacritics (accents) and non-alphabetic characters (punctuation, spaces, etc.), so you only have to construct the multiset representation.

<details>
    <summary>HINT</summary>
    Use the List and Map APIs! The functions groupBy, map, and sorted may come in handy.
</details>

Implement the function `sentenceOccurrences` that computes the occurrence list of an entire sentences.

<details>
    <summary>HINT</summary>
    You can concatenate the words of the sentence into a single word and then reuse the method computeOccurrenceList.
</details>

### Computing anagrams of a word

To compute the anagrams of a word, we use the simple observation that all the anagrams of a word have the same occurrence list. To allow efficient lookup of all the words with the same occurrence list, we will have to *group* the words of the word list according to their occurrence lists.

1. Implement the function `createDictionary` that takes a list of words and generates a dictionary mapping each occurrence list to all corresponding words.

2. Implement the function `wordAnagrams` that returns the list of anagrams of its input word.

### Computing subsets of a multiset

To compute all the anagrams of a sentence, we will need a helper method which, given an occurrence list, produces all the subsets of that occurrence list.

The `subsets` method should return all possible ways in which we can pick a subset of elements from its input. For example, given the occurrence list:

```
List(('a', 2), ('b', 2))
```

the list of all subsets is:

```
List(
    List(),
    List(('a', 1)),
    List(('a', 2)),
    List(('b', 1)),
    List(('b', 2)),
    List(('a', 1), ('b', 1)),
    List(('a', 2), ('b', 1)),
    List(('a', 1), ('b', 2)),
    List(('a', 2), ('b', 2))
  )
```

The order in which you return the subsets does not matter, but the order of elements in each subset does matter: it should be consistent with the original ordering (so, for example, `List(('b', 2), ('a', 1))` is not a valid subset of `List(('a', 2), ('b', 2))`). Note that there is only one subset of an empty multiset, and that is the empty multiset itself.

### Subtracting multisets

We now implement another helper method called `subtract` which, given two multisets `x` and `y`, subtracts the frequencies of the occurrence list `y` from the frequencies of the occurrence list `x`:

For example, given two occurrence lists for words `lard` and `r`:

```scala
val x = List(('a', 1), ('d', 1), ('l', 1), ('r', 1))
val y = List(('r', 1))
```

the result of `subtract(x, y)` is `List(('a', 1), ('d', 1), ('l', 1))`.

The precondition for the `subtract` method is that the occurrence list `y` is a subset of the occurrence list `x` – if the list `y` has some character then the frequency of that character in `x` must be greater or equal than the frequency of that character in `y`. When implementing subtract you can assume that `y` is a subset of `x`.

### Computing anagrams of a sentence

With this, you have all pieces in hand to implement the `sentenceAnagrams`! Start by thinking on paper, and refrain from coding until you have a clear idea of what the algorithm should do.

<details>
    <summary>HINT</summary>
        Using `for` comprehensions helps in formulate the solution of this problem elegantly.
</details>

Test the `sentenceAnagrams` method on short sentences, no more than 10 characters.

The combinations space gets huge very quickly as your sentence gets longer, so the program may run for a very long time if you give it long sentences.

### Application: anagram solver usage guide

To help you enjoy your cool new program, we’ve implemented a command line interface. You can call it from the sbt terminal using the run command. Its syntax is as follows:

```scala
sbt> run [--one-by-one] [--wordlist <name>] <word> [<word>…]
```

Compute sentence anagrams of a collection of words.

Options:

- `--one-by-one`: treat each word individually, printing all known anagrams of each input word.

- `--wordlist <path>`: load an alternate wordlist. Available wordlist are `en-ngsl`, `fr-edu`, `en-debian`, `en-able`, and `fr-lp`.

>**Warning:**
>
>The algorithm does not restrict the size of the sentence you put as an argument. Processing a long sentence will take a lot of time.

We have included the following wordlists:

- `en-ngsl`: New General Word List (2301 English words)

    Source: Browne, C., Culligan, B. and Phillips, J. (2013) [The New General Service List](http://www.newgeneralservicelist.org/)

- fr-edu: Liste eduscol (1458 French words)

    Source: [Liste de fréquence lexicale](https://eduscol.education.fr/186/liste-de-frequence-lexicale), Ministère français de l’éducation nationale et de la jeunesse.

- en-debian: Package wamerican (45376 English words)

    Source: SCOWL, [Spell Checker Oriented Word Lists](http://wordlist.aspell.net/)

- en-able: Enhanced North American Benchmark LExicon (172823 English words)

    Source: [National Puzzlers’ League](https://www.puzzlers.org/)

- fr-lp: Letterpress (336528 French words)

    Source: [Letterpress repository](https://github.com/lorenbrichter/Words)

Here are some examples in word-by-word mode:

```scala
# Small wordlist
> run --one-by-one triangle
altering relating

sbt> run --one-by-one triangle --wordlist en-debian
alerting altering integral relating triangle
```

And here are some in sentence mode:

```scala
sbt> run teacher
he react
he trace
[…]
cat here
the race

sbt> run professeur --wordlist fr-edu
[…]
fou presser
```

### Making it fast (optional) 🔥

The code we wrote above is slow, and not very usable. There are multiple reason for this:

1. Many anagrams are recomputed over and over, recursively, exponentially too many times. This is because there can be many ways to arrive to the same subset, and yet we always recompute it.

2. For short inputs, enumerating all subsets may be faster than testing all possible words. For longer inputs, though, running through all subsets takes a very long time.

3. Our code produces all anagrams, including permutations of the same input. This is not very readable.

4. When two words are anagrams of each other, our word duplicates all sentence anagrams that use the first word to use the second word.

There are fixes for each of these:

1. Use *memoization* — we will learn about this in a few weeks, but if you’re curious you can start using it now!

2. Instead of trying all permutations, try all words; this requires writing a function to check that a multiset is a subset of another one.

3. Print a tree of results instead of a list.

4. Print single-word anagrams on a single line instead of handling them separately.