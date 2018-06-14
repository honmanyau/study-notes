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
* If a question has a big number in it, try it with a smaller number first
* Approach a specific case, and not the great generality, first
* Ask the question "what is the real question?"

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

**Remarks**

After going through the rest of the videos on the puzzle, it seems that the focus is not on solving that problem for now. I'm not sure if proving all other combinations is incorrect is trivial, so I'll leave that for now and stick with the answer to the original question as 4.

### Puzzle 3—The Pirates and the Admirals

3 admirals and 3 pirates walking across a terrain and they come across a river. No bridge, can't swim across because river too wide. Need to use a boat. Boat only has room for 2 people. If the pirates ever outnumber the admiral, bad things happen.

**Summarise key details**

* 3 Admirals
* 3 Pirates
* Boat that can fit 2 people
* How to get everyone across the river without the pirates ever outnumbering the admirals.

**Attempt**

Let admiral be `a` and pirates be `p`. Suppose we start with the representation 3a-3p | 0a-0p | 0a-0p, where the left hand side is the starting point, the middle is the boat, and the right hand side is the other side of the river.

0. 3a-3p | 0a-0p | 0a-0p
1. 2a-2p | 1a-1p | 0a-0p
2. 2a-2p | 1a-0p | 0a-1p
3. 2a-1p | 1a-1p | 0a-1p
4. 2a-1p | 0a-1p | 1a-1p
5. 1a-1p | 1a-1p | 1a-1p
6. 1a-1p | 0a-1p | 2a-1p
7. 1a-0p | 0a-2p | 2a-1p
8. 1a-0p | 0a-1p | 2a-2p
9. 0a-0p | 0a-1p | 3a-2p
9. 0a-0p | 0a-0p | 3a-3p

Done!

**Half way through the the solution video**

Umm... I looks like I assumed that a pirate staying on a boat doesn't count (clearly silly to have thought that)... A better representation is with just two sections and an asterisk indication the location of the boat. Another attempt:

0. `3a-3p *|  0a-0p`
1. `2a-2p  |* 1a-1p`
2. `3a-2p *|  0a-1p`
3. `3a-0p  |* 0a-3p`
4. `3a-1p *|  0a-2p`
5. `1a-1p  |* 2a-2p`
6. `2a-2p *|  1a-1p`
7. `0a-2p  |* 3a-1p`
8. `0a-3p *|  3a-0p`
9. `0a-1p  |* 3a-2p`
10. `0a-2p  *|  3a-1p`
10. `0a-0p   |* 3a-3p`

**Remarks**

Mine was not a bad approach to begin with but I clearly confused myself with the partitioning. :( And it's just a bit creepy that my approach an choice of abbreviation seem to be exactly the same... I swear that was my own work. o___O

### Puzzle 4—Tower of Hanoi

* 3 Pegs
* 8 Discs of different sizes stacked, from smallest to largest going from top to bottom, on one of the pegs

Constraints:

* Only move 1 disc at a time
* Never a bigger disc on top of a smaller disc

Let's try this!

* Note: possible moves do not include the ones that can return the game to an earlier state (combinations, not permutations)

| Turn | Peg A | Peg B | Peg C | Remarks |
| ---- | ----- | ----- | ----- | ------- |
| 1 | 1 2 3 4 5 6 7 8 |  |  | Two possible moves |
| 2a | 2 3 4 5 6 7 8 | 1 |  | One possible move |
| 2b | 2 3 4 5 6 7 8 |  | 1 | Not explored, the solution is probably symmetrical about the two empty pegs at the beginning and if the other route ends up being a solution for moving all to the end, this route is the opposite |
| 3-2a | 3 4 5 6 7 8 | 1 | 2 | Two possible moves |
| 4a-3-2a | 3 4 5 6 7 8 |  | 1 2 | One possible move, taking 4b into account |
| 4b-3-2a | 1 3 4 5 6 7 8 |  | 2 | Dead end—this can only return to 3-* or 4a-* |
| 5-4a-3-2a | 4 5 6 7 8 | 3 | 1 2 | Two possible moves |
| 6a-5-4a-3-2a | 1 4 5 6 7 8 | 3 | 2 | One possible move, taking 6b-* into account |
| 6b-5-4a-3-2a | 4 5 6 7 8 | 1 3 | 2 | Dead end—this can only return to 5-* or 6a-* |
| 7-6a-5-4a-3-2a | 1 4 5 6 7 8 | 2 3 |  | Two possible moves |
| 8a-7-6a-5-4a-3-2a | 4 5 6 7 8 | 1 2 3 |  | One possible move |
| 8b-7-6a-5-4a-3-2a | 4 5 6 7 8 | 2 3 | 1 | `Not yet explored`. Intuition says no. |

Going with intuition at this point:

| Turn | Peg A | Peg B | Peg C | Remarks |
| ---- | ----- | ----- | ----- | ------- |
| 9-8a-* | 5 6 7 8 | 1 2 3 | 4 |  |
| 10 | 5 6 7 8 | 2 3 | 1 4 |  |
| 11 | 2 5 6 7 8 | 3 | 1 4 |  |
| 12 | 1 2 5 6 7 8 | 3 | 4 |  |
| 13 | 1 2 5 6 7 8 |  | 3 4 |  |
| 14 | 2 5 6 7 8 | 1 | 3 4 |  |
| 15 | 5 6 7 8 | 1 | 2 3 4 |  |
| 16 | 5 6 7 8 |  | 1 2 3 4 |  |
| 17 | 6 7 8 | 5 | 1 2 3 4 |  |
| 18 | 6 7 8 | 1 5 | 2 3 4 |  |
| 19 | 2 6 7 8 | 1 5 | 3 4 |  |
| 20 | 2 6 7 8 | 5 | 1 3 4 |  |
| 21 | 6 7 8 | 2 5 | 1 3 4 |  |
| 22 | 6 7 8 | 1 2 5 | 3 4 |  |
| 23 | 3 6 7 8 | 1 2 5 | 4 |  |
| 24 | 3 6 7 8 | 2 5 | 1 4 |  |
| 25 | 2 3 6 7 8 | 5 | 1 4 |  |
| 26 | 1 2 3 6 7 8 | 5 | 4 |  |
| 27 | 1 2 3 6 7 8 | 4 5 |  |  |
| 28 | 2 3 6 7 8 | 4 5 | 1 |  |
| 29 | 3 6 7 8 | 2 4 5 | 1 |  |
| 30 | 3 6 7 8 | 1 2 4 5 |  |  |
| 30 | 6 7 8 | 1 2 4 5 | 3 |  |
| 31 | 1 6 7 8 | 2 4 5 | 3 |  |
| 32 | 1 6 7 8 | 4 5 | 2 3 |  |
| 32 | 6 7 8 | 4 5 | 1 2 3 |  |
| 33 | 4 6 7 8 | 5 | 1 2 3 |  |
| 34 | 4 6 7 8 | 1 5 | 2 3 |  |
| 35 | 2 4 6 7 8 | 1 5 | 3 |  |
| 36 | 1 2 4 6 7 8 | 5 | 3 |  |
| 37 | 1 2 4 6 7 8 | 3 5 |  |  |
| 38 | 2 4 6 7 8 | 3 5 | 1 |  |
| 39 | 4 6 7 8 | 2 3 5 | 1 |  |
| 40 | 4 6 7 8 | 1 2 3 5 |  |  |
| 41 | 6 7 8 | 1 2 3 5 | 4 |  |
| 42 | 1 6 7 8 | 2 3 5 | 4 |  |
| 43 | 1 6 7 8 | 3 5 | 2 4 |  |
| 43 | 6 7 8 | 3 5 | 1 2 4 |  |
| 44 | 3 6 7 8 | 5 | 1 2 4 |  |
| 45 | 3 6 7 8 | 1 5 | 2 4 |  |
| 46 | 2 3 6 7 8 | 1 5 | 4 |  |
| 47 | 1 2 3 6 7 8 | 5 | 4 |  |
| 48 | 1 2 3 6 7 8 | 4 5 |  |  |
| 49 | 2 3 6 7 8 | 4 5 | 1 |  |
| 50 | 3 6 7 8 | 2 4 5 | 1 |  |
| 51 | 6 7 8 | 1 2 4 5 | 3 |  |
| 52 | 6 7 8 | 2 4 5 | 1 3 |  |
| 53 | 2 6 7 8 | 4 5 | 1 3 |  |
| 54 | 1 2 6 7 8 | 4 5 | 3 |  |
| 55 | 1 2 6 7 8 | 3 4 5 |  |  |
| 56 | 2 6 7 8 | 3 4 5 | 1 |  |
| 57 | 6 7 8 | 2 3 4 5 | 1 |  |
| 58 | 6 7 8 | 1 2 3 4 5 |  |  |
| 59 | 7 8 | 1 2 3 4 5 | 6 |  |

Okay, intuition is working, but too tired to be able to verbalise it in a methodic way. Continue tomorrow!

I think that's quite enough to formulate a non-brute force algorithm. Let's try this:

* Represent the disc with numbers from `1` to `8`; `1` being the smallest and `8` being the largest
* Represent the pegs as arrays, with identifiers of `pegA`, `pegB` and `pegC`
* Initially `pegA` has a value of `[1, 2, 3, 4, 5, 6, 7, 8]`, the other arrays are empty, that is `[]`

1. Find an empty array and mark it
2. Compare the first element (index `0`) of the arrays not marked in Step 1 and move the largest number to one of the marked arrays (if available). Comment: if I'm not mistaken, the only time there are two empty spots is at the beginning of the game and at the end of the game, and whether the game ends `pegB` or `pegC` being filled depends on the very first step of the game

    ```
    pegA = [2, 3, 4, 5, 6, 7, 8];
    pegB = [1];
    pegC = [];
    ```

3. Repeat Step 1 and 2 until there are no empty arrays. Comment: At this point, the next one that really needs to be moved next is the largest number, but it can't be moved because any moves involving it is **invalid** (is there a way to determine this without calculating all possibilities, even though there are only two? Or maybe because there are only two it's faster to just try both, and fail, for a computer?)

    ```
    pegA = [3, 4, 5, 6, 7, 8];
    pegB = [1];
    pegC = [2];
    ```

4. [Decision fork 1] Let's continue assuming that one doesn't need to calculate all possibilities. So, no peg is empty, move the smallest number to the peg with the second smallest number (obviously this has to be `unshift`ed into the array)

    ```
    pegA = [3, 4, 5, 6, 7, 8];
    pegB = [];
    pegC = [1 2];
    ```

5. Repeat Step 1 and Step 2... and Step 3. Oh, Step 3 is not actually a Step and should probably be combined with Step 2. Let's leave that for now

    ```
    pegA = [4, 5, 6, 7, 8];
    pegB = [3];
    pegC = [1 2];
    ```

6. Repeat Step 4. Okay, this seems wrong, because 2 needs to be on top of 3 in order for an empty slot to be created.

    ```
    pegA = [4, 5, 6, 7, 8];
    pegB = [1 3];
    pegC = [2];
    ```

Step 4's logic is incorrect. Brain-dump re-attempt:

1. The `target` is to move increasingly higher numbers to an empty slot
2. In order to move a number of higher value, one needs to move the one before it
3. Suppose we start at a `depth` of `0`, the first move's `target` is `0 + 1` = `1`
4. After the first move has been successfully **moved to be the bottom of a peg**, increase `depth++`
5. `depth` is now `1` and `target` is `1 + 1` = `2`
6. Is there an empty slot? Yes—meaning that `2` can be moved to become **the bottommost number of a peg**, `depth++`
7. `depth` is now `2` and `target` is `2 + 1` = `3`
8. Target cannot be moved to the bottom of a peg, **prioritise creating a subtower at the current depth of `2`**
9. Can `depth - 1` be moved? Yes, move `1` to `2`
10. Now there is an empty slot, reprioritise `target` (`3`)

Having a quick look at the initial attempt (the table), it looks like one must always strive to create two subtowers as it is the only way that an empty spot can be created.

1. `depth = 0`, `target = 1`. Find empty slots
    ```
    pegA = [1, 2, 3, 4, 5, 6, 7, 8];
    pegB = [];
    pegC = [];
    ```
2. Empty slots found, referring to the table above, it looks like the subtower just move between `pegB` and `pegC`. Since we want the last subtower (all discs, that is) to be in the center, start with `pegC`
3. Move `target = 1` from `pegA` to `pegC`

    ```
    pegA = [2, 3, 4, 5, 6, 7, 8];
    pegB = [];
    pegC = [1];
    ```
4. Has subtower based on `target = 1` been created? Yes, `target = 2` (don't need `depth` it seems)

    ```
    pegA = [2, 3, 4, 5, 6, 7, 8];
    pegB = [];
    pegC = [1];
    ```

5. `target = 2`. There must be an empty slot because a subtower has just been completed. Move `2` from `pegB` to `pegC`:

    ```
    pegA = [3, 4, 5, 6, 7, 8];
    pegB = [2];
    pegC = [1];
    ```

6. `target = 2`. Target satisified. Treat it as similar to an empty peg and build a subtower of the next smaller size (let's say `subTarget = 2 - 1`) on top of it:

    ```
    pegA = [3, 4, 5, 6, 7, 8];
    pegB = [1, 2];
    pegC = [];
    ```

7. Target satisified. `subTarget = 1 - 1`. All done. Whenever `subTarget = 0` we have an empty slot. Increment target and move new target to empty slot. `target = 3`:

    ```
    pegA = [4, 5, 6, 7, 8];
    pegB = [1, 2];
    pegC = [3];
    ```

8. Try repeating Step 6. Target satisified. `subTarget = 3 - 1`... or not, because you can't move `2`. It seems reasonably to assume that `subTarget` actually starts at `0` and increases by `1` every target is achieved? Let's try `subTarget = 1`. Which slot to move to? If we use the rule established from the beginning and want the current subtower `[1, 2]` to end up on `[3]`, we

    ```
    pegA = [4, 5, 6, 7, 8];
    pegB = [1, 2];
    pegC = [3];
    ```

Okay, this looks like recursion. Let's try coding:

```javascript
const pegA = [2, 3, 4, 5, 6, 7, 8];
const pegB = [];
const pegC = [1];
let counter = 0;

function solvePuzzle(pegs, list = [1]) {
  if (pegB.toString() === '1,2,3,4,5,6,7,8') {
    console.log(pegs);
    console.log(`pegA: [${pegA}], pegB: [${pegB}], pegC: [${pegC}]`);
    console.log('Done!');
    return;
  }
  else {
    counter++;
    if (counter > 250) {
      return;
    }
  }

  pegs.sort(function(a, b) {
    return (a[0] || 0) - (b[0] || 0);
  });

  let newList = list.slice();
  let sourcePeg, targetPeg;

  console.log(pegs);

  sourcePeg = pegs[2];
  targetPeg = pegs.filter((peg) => !peg[0])[0];
  newList.push(sourcePeg[0]);
  targetPeg.unshift(sourcePeg.shift());

  for (let i = 0; i < list.length; i++) {
    counter++;
    console.log(pegs);

    let disc = list[i];

    sourcePeg = pegs.filter((peg) => peg[0] === disc)[0];
    targetPeg = pegs.filter((peg) => {
      return (
        peg[0] !== disc &&
        ((peg[0] > disc && (peg[0] - disc) % 2) || !peg[0])
      );
    })[0];

    targetPeg.unshift(sourcePeg.shift());
    newList.push(disc);
  }

  solvePuzzle(pegs, newList);
}

solvePuzzle([pegA, pegB, pegC]);
```

Ended up noticing an easier pattern when coding! This is how it works:

1. Keep track of three arrays, each corresponds to one of the begs. Starting with [2, 3, 4, 5, 6, 7, 8], [], and [1] to avoid selecting the alternative solution
2. Keep a `list` of moves made so far (starting at `[1]` because of the preconfiguration mentioned above)
3. Make a copy of `list` and assign it to `newList`
4. A the beginning of each loop, move the largest number at the top of any peg, for example, the first loop is `2`
5. Append the number of the disc that has just been move to `newList`
6. Go through each of the items in `list`, move the disc with the number corresponding to each of the number to the peg with a number at the top that is:

    * Not equal to the current number picked from `list`
    * Is larger than the current number picked from `list` **AND** their difference is an odd number
    * If second condition cannot be met and an empty peg is available, pick the empty peg
7. Replace `list` with `newList`
8. Repeat Step 4 to Step 6 until the required solution is found

If we mark the moves made during Step 4 with an asterisk, `list` grows as follows:

* 1\*
* 1 2\* 1
* 1 2 1 3\* 1 2 1
* 1 2 1 3 1 2 1 4\* 1 2 1 3 1 2 1
* 1 2 1 3 1 2 1 4 1 2 1 3 1 2 1 5\* 1 2 1 3 1 2 1 4 1 2 1 3 1 2 1 ... etc.

## Questions

## Resources

* [The Heart of Mathematics: An Invitation to Effective Thinking, 4th Edition](https://www.wiley.com/en-us/The+Heart+of+Mathematics%3A+An+Invitation+to+Effective+Thinking%2C+4th+Edition-p-9781118156599)
* [The 5 Elements of Effective Thinking](https://press.princeton.edu/titles/9810.html)
