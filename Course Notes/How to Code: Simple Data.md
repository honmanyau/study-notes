# How to Code: Simple Data

## Table of Contents

* [How to Code: Simple Data](#how-to-code-simple-data)
  * [Notes](#notes)
  * [Exercises](#exercises)
  * [Questions](#questions)

## Notes

### Module 0: Introduction

* It's hard to write a program because it's usually given as a poorly-formed problem
* Figure out exactly what it is that we want the program to do first
* Then learn to break down the problem into well-choosen pieces first that can be well-tested and maintained and modified
* BSL (Beginning Student Language)

### Module 1a: Beginning Student Language

#### Expressions

```BSL
(sqrt (+ (sqr 3) (sqr 4)))
```

* Inexact numbers are prefixed with `i` in the output

#### Quiz Set 1

Was caught off guard by question 3—what's shown in the output is, of course also an expression, was too focused on the input.

#### Peer Question

**Answer**:

The **intent** is the clearest in this case. The average of n numbers is the sum of those n numbers divided by n—pre-computing part of the sum, as in the case of `(/ (+ -8 6.2) 3)` , obscures this the intention of **comput[ing] the average of 4, 6.2 and -12**. In addition, `(/ (+ -8 6.2) 3)`, or simply giving the answer `-0.6` are not generalisable and therefore not reusable.

**Reflection**:

No change.

#### Evaluation

* Evaluation goes from left to right, inside to outside

#### Strings and Images Part 1

* `string-append`
* `string-length`
* `substring`

#### Strings and Images Part 2

* `require 2htdp/image`
* `circle <radius> <fill-mode> <colour>`
* `rectangle <width> <height> <fill-mode> <colour>`
* `text <string> <size> <colour>`

Grouping images together/positioning:
* `above`
* `beside`
* `overlay`

#### Discussion: Create Your Own Image

```BSL
(require 2htdp/image)
(crop/align "left" "top" 160 80
  (overlay
    (circle 20 "solid" "purple")
    (circle 30 "solid" "blue")
    (circle 40 "solid" "green")
    (circle 50 "solid" "forestgreen")
    (circle 60 "solid" "yellow")
    (circle 70 "solid" "orange")
    (circle 80 "solid" "red")
  )
)
```

#### Constant Definitions

* `define <identifier> <expression>`
* `rotate <degrees> <image>`

Images can be defined by copy and pasting it as an expression, too!

#### Function Definitions

* ```
  (define (<name> <parameter>)
    <expression>
  )
  ```

#### Booleans and if Expressions

* `string=? <string> <string>`
* `img-width <img>`
* `img-height <img>`
* ```
  (if <expression> <true expression> <else expression>)
  ```
* `and` (`&&`)
* `or` (`||`)
* `not`

#### Using the Stepper

#### Discovering Primitives

* One could `Ctrl` + click or right click on a primitive to see its doc in the Help Disk

#### Recommended Problems

**BSL P16—Foo Evaluation**

```
(define (foo s)
  (if (string=? (substring s 0 1) "a")
      (string-append s "a")
      s))

(foo (substring "abcde" 0 3))

(foo "abc")

(if (string=? (substring "abc" 0 1) "a")
    (string-append "abc" "a")
    "abc")

(if (string=? "a" "a")
    (string-append "abc" "a")
    "abc")

(string-append "abc" "a")

"abca"
```

#### Remarks/Note to Self

Took just shy of 2 hours to go through all of the material by watching at 2x speed, while completing all quizzes and problems in between. Only attempted the "most difficult" of the recommend since.

### 1b: How to Design Functions

#### Full Speed HtDF Recipe

* `check-expect`

1. Signature, purpose and stub
2. Define examples, wrap each in check-expect
3. Template and inventory
4. Code the function body
5. Test and debug until correct

**Remarks**: TDD!

#### Slow Motion HtDF Recipe

#### A First HtDF Problem

>  Problem: Design a function that pluralizes a given word. (Pluralize means to convert the word to its plural form.) For simplicity you may assume that just adding s is enough to pluralize a word.

Attempt:

```BSL
;; String -> String
;; Add 's' to the end of a the given string

(check-expect (pluralise "cat") "cats")
(check-expect (pluralise "nyanpasu") (string-append "nyanpasu" "s"))

;; (define (pluralise str) "") // Stub
;; (define (pluralise str) (... str)) // Template

(define (pluralise str) (string-append str "s"))
```

* Code coverage—how much of the code is actually tested. For example, if a return value is never tested, it means that there are not enough tests (coverage not perfect).


#### Recommended Problems

**HtDF P2 - Less Than 5**

> Design a problem to check if the length of a string is less than 5.

Attempt:

```BSL
;; String -> String // Signature
;; Check the length of a string, return true if it is less than 5, otherwise return false // Purpose
(check-expect (str-len-less-than-5? "nyanpasu") false)
(check-expect (str-len-less-than-5? "cat") true)

;; (define (str-len-less-than-5? str) "") // Stub
;; (define (str-len-less-than-5? str) (... str)) // Template

(define (str-len-less-than-5? str)
  (if (< (string-length str) 5)
      true
      false
  )
)
```

**HtDF P3 - Boxify**

> Design a function to put a box around a given image.

Attempt:

;; Image -> Image // Signature
;; Take an image, gets its height and width in pixels, overlay it on top of a rectangle that has an outline of 1px // Purpose
;; (define (boxify img) IMAGE) // Stub
(check-expect (image-width (boxify IMAGE)) (+ (image-width IMAGE) 1))
(check-expect (image-height (boxify IMAGE)) (+ (image-height IMAGE) 1))


;; (define (boxify img) (
;;   (... (... (image-width img) (... (image-height img)))
;; )) // Template

(define (boxify img)
  (overlay
    img
    (rectangle (+ (image-width img) 1) (+ (image-height img) 1) "outline" "black")
  )
)

**HtDF P6 - Double Error**

> Fix the error(s) in a function that doubles a given number.

Attempt:

```BSL
;; Number -> Number
;; doubles n
(check-expect (double 0) 0)
(check-expect (double 4) 8)
(check-expect (double 3.3) (* 2 3.3))
(check-expect (double -1) -2)

;; (define (double n) 0) ; stub

(define (double n)
  (* 2 n))
```

### Quiz

```BSL
;; Signature:
;; Image -> Boolean

;; Purpose:
;; Take two images, say imgA and imgB, as inputs; calculate their areas
;; return true if the area of imgA is greater than imgB
;; otherwise return false

(check-expect (img-larger? (square 20 "solid" "black") (square 10 "solid" "black")) true)
(check-expect (img-larger? (square 10 "solid" "black") (square 20 "solid" "black")) false)

;; Stub:
;; (define (img-larger? imgA imgB) false)

;; Template:
;; (define (img-larger? imgA imgB)
;;  ((... imgA) (... imgB))
;;  )

(require 2htdp/image)

(define (img-larger? imgA imgB)
  (>
   (* (image-width imgA) (image-height imgA))
   (* (image-width imgB) (image-height imgB)))
  )
```

Overloooked the an additional `Image` in the signature. Signature may or may not be entirely appropriate, I think I would have used `(... (... imgA) (... imgB))` (with those extra ellipsis at the beginning); according to the evaluation tutorial video, `(... imgA imgB)` is considered good already.

It is worth noting that the problem appears to be ambiguous. Larger could be perceived differently depending on what measures we are talking about.

I actually considered writing more tests and the video suggests 9 if a successful test is that both dimensions of `imgA` is larger than those of `imgB`. However, I would argue that it should be 4 because it's either larger or not larger; equal already means that it's not larger and is implied by the `>` operator. Given that I implemented an area test, instead of both of the dimensions individually, I feel that the 2 tests is sufficient.

### Module 2: How to Design Data

#### cond Expressions

* `cond [<expression> <expression>]` (like a `switch`, except that each case takes a question and an answer)

#### Data definitions

* Code that describes how information in the problem domain is mapped (represented/interpreted) by data in the program.

#### Structure of Information Flows Through

> Identifying the structure of the information is a key step in program design

> Information -> Data -> template -> Functions, Tests

#### Peer Question

Attempt:

```
Without more information as to what the data type is used for, I think both are reasonable implementations in this particular case. The enumeration type comments excel at being semantic and the information-data mapping is therefore tighter.

The interval method is good, but different, in the sense that it translates the information to data from a mathematical point of view—in cases where the data type is used for more than just identification, for example, where additional operations are performed based on the type of polygon is involved, the interval definition may be superior. Perhaps worth noting is that the interval type is is also easier to extend and, on that note, the semantic value of the enumeration method also decreases as the size of a regular polygon grows.
```

#### Recommended Problems

**HtDD P2 - Demolish**

Attempt:

```BSL
;; =================
;; Data definitions:
;; =================

;; Building is one of:
;; * "heritage"
;; * "old"
;; * "new"
;;
;; interpret a building is "heritage", "new" or "old" depending on how old they are
#;
(define (get-building-status status)
  (cond [(string=? "new" status) (...)]
        [(string=? "old" status) (...)]
        [(string=? "heritage" status) (...)]))

;; =================
;; Functions:
;; =================

;; Signature:
;; BuildingStatus -> Boolean

;; Stub
;; (define (tear-down-building status) false)

;; Purpose:
;; Return true, meaning that a building should be torn down, if a building's classification is old

(check-expect (tear-down-building "old") true)
(check-expect (tear-down-building "new") false)
(check-expect (tear-down-building "heritage") false)

;; Template:
;; Using template from data definition
#;
(define (tear-down-building status)
  (cond [(string=? "old" status) true]
        [(string=? "new" status) false]
        [(string=? "heritage" status) false]))

;; Simplified attempt
(define (tear-down-building status)
  (cond [(string=? "old" status) true]
        [else false]))
```

**HtDD P3 - Rocket Descent**

Attempt:

```BSL
;; =================
;; Data definitions:
;; =================

;; This is an itemisation problem because there are three states with different types of values
;; RocketDescent is one of:
;; * Number (0, 100]
;; * false
;;
;; interp. a number between 0–100 (not including 0) means that the rocket is
;; within 100 km to touchdown and has not touch down; else (distance is 0) the
;; rocket has has touched down

(define RD1 100)
(define RD2 50)
(define RD3 1)
(define RD4 false)

#;
(define (fn-for-rocket-descent dist)
  (cond [(and (number? dist) (<= dist 100) (> dist 0)) (... dist)]
        [else (...)]))

;; =================
;; Functions:
;; =================

;; Signature:
;; RocketDescent -> String
;;
;; Purpose:
;; Return "Rocket is descending" when the rocket is not withing 100 km
;; Return "Distance to touchdown: [dist]" when it is within 100 km but not 0 km
;; Return "The rocket has landed!" if touched down.
;;
;; Stub:
;;(define (rocket-descent-to-msg dist) "")

;; Tests:
(check-expect (rocket-descent-to-msg 100) "Distance to touchdown: 100")
(check-expect (rocket-descent-to-msg 50) "Distance to touchdown: 50")
(check-expect (rocket-descent-to-msg 1) "Distance to touchdown: 1")
(check-expect (rocket-descent-to-msg false) "The rocket has landed!")

;; Using template from RocketDescent definition

(define (rocket-descent-to-msg dist)
  (cond [
         (and (number? dist) (<= dist 100) (> dist 0))
         (string-append "Distance to touchdown: " (number->string dist))
        ]
        [
         else
         "The rocket has landed!"
        ]
  )
)
```

#### Quiz

Attempt:

```BSL
;; HtDD Design Quiz

;; Age is Natural
;; interp. the age of a person in years
(define A0 18)
(define A1 25)

#;
(define (fn-for-age a)
  (... a))

;; Template rules used:
;; - atomic non-distinct: Natural


; Problem 1:
;
; Consider the above data definition for the age of a person.
;
; Design a function called teenager? that determines whether a person
; of a particular age is a teenager (i.e., between the ages of 13 and 19).


;; Signature:
;; Age -> Boolean

;; Purpose:
;; Return true if the input is a natural number between 13 and 19, inclusive.
;; Otherwise return false.

;; Stub:
;; (define (teenager? age) false)

;; Tests/examples:
(check-expect (teenager? 12) false)
(check-expect (teenager? 13) true)
(check-expect (teenager? 16) true)
(check-expect (teenager? 19) true)
(check-expect (teenager? 20) false)

;; Template rule used (see also template for Age):
;; * Atomic Non-distinct: Natural
;; (define (teenager? age) (... age))

(define (teenager? age)
  (and (>= age 13) (<= age 19))
  )

; Problem 2:
;
; Design a data definition called MonthAge to represent a person's age
; in months.


;; MonthAge is Natural
;; interp. a person's age in number of months

(define ONE 12)
(define TWELVE 144)

#;
(define (fn-for-age age)
  (... age)
)

;; Template rules used:
;; * Atomic non-distinct: Natural

; Problem 3:
;
; Design a function called months-old that takes a person's age in years
; and yields that person's age in months.
;


;; Signature:
;; MonthAge -> Natural

;; Purpose:
;; Takes a person's age in years, return the age times 12

;; Stub;
;;(define (months-old age) 0);

;; Tests/examples;
(check-expect (months-old 1) 12);
(check-expect (months-old 12) 144);

;; Template (see template for Age):
#;
(define (months-old age)
  (... age)
)

(define (months-old age)
  (* age 12)
)

; Problem 4:
;
; Consider a video game where you need to represent the health of your
; character. The only thing that matters about their health is:
;
;   - if they are dead (which is shockingly poor health)
;   - if they are alive then they can have 0 or more extra lives
;
; Design a data definition called Health to represent the health of your
; character.
;
; Design a function called increase-health that allows you to increase the
; lives of a character.  The function should only increase the lives
; of the character if the character is not dead, otherwise the character
; remains dead.


;; =================
;; Data definition:
;; =================

;; Health is one of:
;; * Natural
;; * false
;;
;; interp. if false the character is dead (0 health),
;; otherwise a natural number represents the amount of extra lives

(define H1 42)
(define H2 false)

;; Template rules used:
;; * One of (itemisation) 2 cases
;; * Atomic non-distinct: Natural
;; * Atomic distinct: false
(define (Health hp)
  (cond [(number? hp) (... hp)]
        [else (...)]
  )
)

;; =================
;; Function:
;; =================

;; Signature:
;; Health -> Boolean

;; Purpose:
;; If the character is not dead, return true (allow lives to be increased);
;; otherwise, return false (lives cannot be incresed because character is dead)

;; Stub:
#;
(define (increase-health hp)
  false
)

;; Tests/Examples
(check-expect (increase-health false) false)
(check-expect (increase-health 0) true)
(check-expect (increase-health 42) true)

;; Template (see definition for Health):
;; * One of (itemisation) 2 cases
;; * Atomic non-distinct: Natural
;; * Atomic distinct: false
#;
(define (increase-health hp)
  (cond [(number? hp) (... hp)]
        [else (...)]
  )
)

(define (increase-health hp)
  (cond [(number? hp) true]
        [else false]
  )
)
```

**Remarks after watching evaluation video**

The design specification is ambiguous in what should be implemented in the function for Problem 4:

> Design a function called increase-health that allows you to increase the
lives of a character. The function should only increase the lives
of the character if the character is not dead, otherwise the character
remains dead.

I implemented `Health -> Boolean` as it matches what the first sentence says and it doesn't specify whether a boolean (allowed or not), the amount of health to be added (`Natural`) or the new health (`Health`) should be returned; but it seems that they were expecting `Health -> Health`.

## Questions
