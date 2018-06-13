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
2. Use a balance once and and find out (by averaging the weights) that the stone is in the pile of 5
3. Divide the pile of 5 stones again into one pile of 2 stones and one pile with 3 stones, use the remaining balance, and find out that the stone is in the pile with 3 stones
4. The probability of picking the one containing the jewel is 1/3

Can't immediately think of a better solution to maximise Allie's chance.

## Questions

## Resources

* [The Heart of Mathematics: An Invitation to Effective Thinking, 4th Edition](https://www.wiley.com/en-us/The+Heart+of+Mathematics%3A+An+Invitation+to+Effective+Thinking%2C+4th+Edition-p-9781118156599)
* [The 5 Elements of Effective Thinking](https://press.princeton.edu/titles/9810.html)
