# Effective Thinking Through Mathematics

## Table of Contents

* [Effective Thinking Through Mathematics](#html)
  * [Notes](#otes)
  * [Puzzles](#puzzles)
  * [Questions](#questions)

## Notes

### 5 Elements of Effective Thinking (Introduction)

* Understand simple things deeply
* Make mistakes
* Raise questions ("What's missing?", "Can this be extended?")
* Follow the flow of ideas
* Change

### Strategies for Solving Puzzles

* Ignoring the superfluous details and get to the essence
* Don't rush
* List all the possibilities methodically, including the incorrect ones, and articulate **why** the incorrect ones are incorrect

## Puzzles

### Puzzle 1—That's a Meanie Genie

Allie discovered an ancient oil lamp near an archaeological dig near the highlands of Tibet. Allie rubbed the lamp and a genie named Murray appeared with a puff of Magenta smoke.

Murray said that he can grant Allie 3 wishes.

* Ramanujaun(?) the jewel that was discovered by Hardy the high llama (!?), 9 idental stones appeared. Allie is only allowed to choose one, which contains the jewel. They look the same, but the one with the jewel weights slightly more.
* Allie said she wished that she had a scale and it appeared.

Murray was locked up for 1709 years.

* Tip: scale can only be used once.
* Allie exclaimed that she wishes that she has another scale, and it appeared.

Is it possible for Allie to select the slightly heavier stone using just the two scales? Explain why or why not.

**Brain-dumping Attempt**

It's definitely possible, but how probable is another matter. The first thing that comes to mind for maximising the probability is a binary search. Assuming the worst case scenario:

1. Divide the pile into 2 piles, one containing 5 stones and the other 4
2. Use a balance once and find out which pile has a heavier averaged weight. The worse outcome is that the stone is in the pile of 5
3. Divide the pile of 5 stones again into one pile of 2 stones and one pile with 3 stones, use the remaining balance, and find out that the stone is in the pile with 3 stones
4. The probability of picking the one containing the jewel is 1/3

Can't immediately think of a better solution to maximise Allie's chance.

**After watching the first walkthrough**

Ah, one could put one to the side to start with, that will lead to a worst case scenario of 50% instead of 1/3.

**Random thought**

What does weighing once mean? Can does that mean putting things down AND removing them? :p

**SCALE!?**

Waaaaah, we are talking about those scales!? **FACEPALM**

Oh... It's actually drawn earlier. o^O

**Conclusion**

* Don't rush
* List all the possibilities methodically, including the incorrect ones, and articulate **why** the incorrect ones are incorrect
* That was embarrassing, do better next time!

### Puzzle 2—A Shaky Story

Stacy and Sam (Smith).

Party, five couples. Shook hands.

* Nobody shook hands with herself/himself
* Nobody shook hands with their own spouse

Sam asked each of the nine people at the end of the night how many hands have each of them shaken, and each of them gave a different answer. That is 0, 1, 2, 3, 4, 5, 6, 7, 8. Determine the number of times that Stacy shook hands with other people.

**Can you pose a simpler question that would be related, but much simpler?**

Suppose there is only one other couple other than Stacy and Sam. Then Sam asks the other 3 at the end of the night and the answers were 0, 1 and 2.

Suppose the other 2 guests are labelled as X and Y:

| Stacy | X | Y |
| ----- | - | - |
| 2     | 1 | 0 |
| 2     | 0 | 1 |
| 1     | 2 | 0 |
| 1     | 0 | 2 |
| 0     | 2 | 1 |
| 0     | 1 | 2 |

It **seems** like **ALL** solutions are valid. *Scratch head*

Double check:

* Stacy cannot shake hands with Sam and herself, but can shake hands with X or Y—maximum of 2 and minimum of 0
* X cannot shake hands with X or Y, but can shake hands with Sam or Stacy—maximum of 2 and minimum of 0
* Y cannot shake hands with Y or X, but can shake hands with Sam or Stacy—maximum of 2 and minimum of 0

Am I missing something or misunderstanding something? If that is indeed the case then even as the number of couples attended increases, this will stay true.

Oh, I'm an idiot, another attempt:

* Stacy shakes hands with 2 people, X and Y must shake at least 1 hand, but that's impossible since either X or Y shook hand 0 times
* Stacy shakes hands with 1 person, say X, X shakes hand 2 times (Stacy and Sam) and Y shakes hands 0 times [okay]
* Stacy shakes hands with nobody, X or Y must shake hands with 2 people—but that's impossible because Stacy is out

Three couples—Stacy, Sam, A, B, X, Y (game pad?)

* Stacy 4 times, meaning A, B, X and Y must shake hands at least 1 times, but that's impossible since someone shook hands 0 times
* Stacy 3 times (B, X, Y), A 0 times, B 1 time (Y), X 2 times (Stacy, Sam), Y 4 times (Stacy, Sam... doesn't work)


Wait, sudden thought: in the simpler example the sum of all hand shakes between couple is 2. This sum, for any number of couples, can be (I think) calculated as (numberOfCouples - 1) * 2; 2 couples is 2, 3 couples is 4 and, indeed, 5 couples is 8 as given.

If we continue with this conjecture, and since it doesn't matter how many times Sam shakes hands with people (between 0 to the maximum number for a given party size), the number of times that Sam has to shake hands in the 3-couple case is:

4 * 3 - 4 - 3 - 2 - 1 = 12 - 10 = 2

So Stacy has to shake hands 2 times. In that scenario, we get left with 0, 1, 3, 4, and the only combination is 1-3 and 0-4. Let's try that:

Stacy 2 times (B, Y), A 0 time, B 4 times (Stacy, Sam, X, Y), X 1 time (B), Y 3 times (Stacy, Sam, B), Sam (B, Y)

I haven't proven that the other combinations are wrong yet, but this seems to work!

## Questions

## Resources

* [The Heart of Mathematics: An Invitation to Effective Thinking, 4th Edition](https://www.wiley.com/en-us/The+Heart+of+Mathematics%3A+An+Invitation+to+Effective+Thinking%2C+4th+Edition-p-9781118156599)
* [The 5 Elements of Effective Thinking](https://press.princeton.edu/titles/9810.html)
