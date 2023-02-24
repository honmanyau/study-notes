# How to Code: Simple Data

## Table of Contents

* [Programming Languages, Part A](#programming-languages-part-a)
  * [Disclaimer](#disclaimer)
  * [Progress](#progress)
  * [Notes](#notes)
  * [Resources](#resources)

## Disclaimer

Everything in this file is derived from going through [Programming Languages, Part A](https://www.coursera.org/learn/programming-languages) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)).


## Progress

| Title    | Time spent | Score   | Remarks                                                  |
| -------- | ---------- | ------- | -------------------------------------------------------- |
| Module 1 | 3 hours    | 100/100 | Super sadface about time wasted on macOS installation. 😰 |
| Module 2 | 3 hours    |         | In progress                                              |


## Notes

### Module 1

#### Introductory reading and videos (1 hour)

- Goal: learning the core ideas around how all programming languages built.
- Understand ideas we use when we program and how they are expressed in languages.
- Yay, it's going to be lots of functional programming!

#### Part A Software Installation and Use: SML and Emacs (1 hour of failure on macOS and 30 minutes on Windows)

- Installing SML/NJ was a bit painful on macOS, and in the end I still ended up with a segmentation fault when I ran `1 + 1;`. In favour of actually doing something actually useful I decided to just switch to my Windows machine.


#### Practice Programming Assignment: Homework 0 (30 minutes)

- [Solution (100/100)](./homework-0/homework-0-solution.sml)
- Peer-reviewed 3 solutions. ☀️🎉


### Module 2

Reading for this section (login required): https://www.coursera.org/learn/programming-languages/supplement/CKUfg/section-1-reading-notes

#### SML Syntax

- Comments: `(* *)`.
- Variable binding:

  ```sml
  val x = 42;
  ```
- Importing bindings from a files: `use filename.sml`.
- Division operator: `div`.
- Functions:

  ```sml
  fun cube(x : int) = x * x * x;
  ```
- Pair/tuples:

  ```sml
  val pair = (2, 4);
  val v1 = #1 pair;
  val v2 = #2 pair;
  ```
- List: `[1, 2, 3, 4]`
- Cons operator: `5::[6, 7, 8] (* [5, 6, 7, 8] *)`

#### ML Variable Bindings and Expressions
- Syntax: how something is written.
- Semantics: what something means.
  - Type-checking (before program runs).
  - Evaluation (as program runs).

#### Rules for Expression

1. Syntax
2. Type-checking rules.
3. Evaluation rules.

Exercise: what are the syntax, type-checking rules, and evaluation rules of less than?

- Syntax: if `e1 < e2`, where two expressions is separated by the `<` symbol.
- Type-checking rule: both `e1` and `e2` must be of type `int` or `float` (not sure what other number types there are in SML), and `e1 < e2` has type `bool`.
- Evaluation rules: if `e1` is smaller than `e2`, the expression evaluates `true`; if `e1` is greater in larger than `e2`, the expression evaluates `false`.

#### The REPL and Errors

#### Shadowing

- Multiple bindings to the same variable.
- Often poor style.
- Multiple `use`s in the same file can cause unexpected shadowing in ML.

#### Functions Informally

#### Functions Formally

- "A function is already a value!"
- Function _binding_ and function _calling_ have separate syntax, type-checking, and evaluation rules.

#### Pairs and Other Tuples

- Pair example:

```sml
fun swap(pair: int * bool) = (#2 pair, #1 pair);
```

- Pair and tuples have predefined sizes.

#### Introducing Lists

- No limit to size, but all elements must be the same type.

#### List Functions

- Recursions are heaps funnnnn.

## Resources

- [Some default Emacs key bindings (login required)](https://www.coursera.org/learn/programming-languages/supplement/mi5oU/part-a-software-installation-and-use-sml-and-emacs)