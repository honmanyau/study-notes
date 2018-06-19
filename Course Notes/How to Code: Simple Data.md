# How to Code: Simple Data

## Table of Contents

* [How to Code: Simple Data](#how-to-code-simple-data)
  * [Notes](#notes)

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

### Module 3a: How to Design Worlds

#### Recommended Problems

Did not attempt the recommend problems because I was already familiar with most of the concepts as I make my own games in JavaScript and haven't learnt anything new in this module. There is also going to be the quiz.

### Module 3b: Compound Data

* `define-struct <structure identifier> (<property name> <property name>...)`
* Constructor call: `define <identifier> (make-<structure identifier> <arguments>)`
* Property/key access: `<structure-identifier>-<property name>`

#### Recommended Problems

Skipped recommended problems again because it is starting to get repetitive and I feel that there is little to gain from going going through this. It's not that I don't think any of this is important, but having programmed for over a year and a half now, having gone through the the exercises in the first two modules diligently, having done TDD, and having started programming in TypeScript for a short while now; at this point it feels like I'm just spending time on learning new syntax in a language that I most likely won't be productive in is really far from ideal at a time where I have a pile of things to learn, too.

#### Quiz

Attempt:

```BSL
(require 2htdp/image)
(require 2htdp/universe)

;; =================
;; Constants:
(define STAGE-WIDTH 400)
(define STAGE-HEIGHT 400)
(define STAGE-CTR-X (/ STAGE-WIDTH 2))
(define STAGE-CTR-Y (/ STAGE-HEIGHT 2))

(define RADIUS 25)
(define SPEED 2)

(define MTScene (rectangle STAGE-WIDTH STAGE-HEIGHT "solid" "black"))

;; =================
;; Data definitions:

(define-struct bloom (r x y dx dy colour))
;; BloomState is (make-bloom Number Number Number Number Number String)
;; interp. The current state of bloom, which is... well, just a circle
;; but I obviosuly can't name it circle due to identifier conflict
;; * r is the radius of the circle
;; * x is the x-coordinate of the center of the circle
;; * y is the y-coordinate of the center of the circle
;; * dx is the current x velocity of the circle
;; * dy is the current y velocity of the circle
;; * colour is... obviously the colour! Only if we were encouraged to use meaningful variable names to begin with...

(define BLOOM1 (make-bloom 25 200 200 1 1 "blue"))
(define BLOOM2 (make-bloom 25 26 26 1 1 "pink"))

;; Template rules used:
;; * Compound: 6 fields
#;
(define (fn-for-bloom-state bloom)
  (... (bloom-r bloom)
       (bloom-x bloom)
       (bloom-y bloom)
       (bloom-dx bloom)
       (bloom-dy bloom)
       (bloom-colour bloom)
  )
)

;; =================
;; Functions:

;; BloomState -> BloomState
;; Main function that is also the game loop
;; Start the world with (main (make-bloom RADIUS (/ STAGE-WIDTH 2) RADIUS SPEED SPEED "pink"))
(define (main bloom)
  (big-bang bloom                   ; WS
            (on-tick update)     ; WS -> WS
            (to-draw render)   ; WS -> Image
           ; (on-mouse ...)      ; WS Integer Integer MouseEvent -> WS
           ; (on-key ...)      ; WS KeyEvent -> WS
  )
)

;; BloomState -> BloomState
;; Return the x-velocity of the next frame: if at the left and right edge, return dx = -dx, otherwise return dx
;; Right edge:
(check-expect
  (next-dx (make-bloom RADIUS (- STAGE-WIDTH RADIUS 1) (- STAGE-HEIGHT 200) SPEED SPEED "pink"))
  (* SPEED -1)
)
;; Left edge:
(check-expect
  (next-dx (make-bloom RADIUS (- RADIUS SPEED) (- STAGE-HEIGHT 200) (* SPEED -1) SPEED "pink"))
  SPEED
)
;; Center:
(check-expect
  (next-dx (make-bloom RADIUS (- STAGE-WIDTH 200) (- STAGE-HEIGHT 200) SPEED SPEED "pink"))
  SPEED
)

;; Stub:
;; (define (next-dx bloom) 1)

;; Template:
#;
(define (next-dx bloom)
  (... bloom)
)

(define (next-dx bloom)
  (if
    (or
      (>= (+ (+ (bloom-x bloom) (bloom-r bloom)) (bloom-dx bloom)) STAGE-WIDTH)
      (<= (+ (- (bloom-x bloom) (bloom-r bloom)) (bloom-dx bloom)) 0)
    )
    (-(bloom-dx bloom))
    (bloom-dx bloom)
  )
)

;; BloomState -> Integer
;; Return the y-velocity of the next frame: if at the left and right edge, return dy = -dy, otherwise return dy
;; Right edge:
(check-expect
  (next-dy (make-bloom RADIUS (- STAGE-WIDTH 200) (- STAGE-HEIGHT RADIUS 1) SPEED SPEED "pink"))
  (* SPEED -1)
)
;; Left edge:
(check-expect
  (next-dy (make-bloom RADIUS (- STAGE-WIDTH 200) (- RADIUS SPEED) SPEED (* SPEED -1) "pink"))
  SPEED
)
;; Center:
(check-expect
  (next-dy (make-bloom RADIUS (- STAGE-WIDTH 200) (- STAGE-HEIGHT 200) SPEED SPEED "pink"))
  SPEED
)

;; Stub:
;; (define (next-dy bloom) 1)

;; Template:
#;
(define (next-dy bloom)
  (... bloom)
)

(define (next-dy bloom)
  (if
    (or
      (>= (+ (+ (bloom-y bloom) (bloom-r bloom)) (bloom-dy bloom)) STAGE-WIDTH)
      (<= (+ (- (bloom-y bloom) (bloom-r bloom)) (bloom-dy bloom)) 0)
    )
    (-(bloom-dy bloom))
    (bloom-dy bloom)
  )
)

;; BloomState -> BloomState
;; Move bloom accoding to its velocities, and recalculate dx and dy using the next-dx and next-dy helper functions
;; Top-left
(check-expect
  (update (make-bloom RADIUS (+ RADIUS SPEED) (+ RADIUS SPEED) (* SPEED -1) (* SPEED -1) "pink"))
  (make-bloom RADIUS RADIUS RADIUS SPEED SPEED "pink")
)
;; Bottom-right
(check-expect
  (update (make-bloom RADIUS (- (- STAGE-WIDTH RADIUS) SPEED) (- (- STAGE-HEIGHT RADIUS) SPEED) SPEED SPEED "pink"))
  (make-bloom RADIUS (- STAGE-WIDTH RADIUS) (- STAGE-HEIGHT RADIUS) (* SPEED -1) (* SPEED -1) "pink")
)
;; Center
(check-expect
  (update (make-bloom RADIUS (+ (- STAGE-WIDTH 200) SPEED) (+ (- STAGE-HEIGHT 200) SPEED) SPEED SPEED "pink"))
  (update (make-bloom RADIUS (+ (- STAGE-WIDTH 200) SPEED) (+ (- STAGE-HEIGHT 200) SPEED) SPEED SPEED "pink"))
)

;; Template from BalloonState
(define (update bloom)
  (make-bloom
    (bloom-r bloom)
    (+ (bloom-x bloom) (bloom-dx bloom))
    (+ (bloom-y bloom) (bloom-dy bloom))
    (next-dx bloom)
    (next-dy bloom)
    (bloom-colour bloom)
  )
)

;; BloomState -> Image
;; Render blooms onto the MTScene
(check-expect
  (render (make-bloom RADIUS (- STAGE-WIDTH 200) (- STAGE-HEIGHT 200) SPEED SPEED "pink"))
  (place-image
    (circle RADIUS "solid" "pink")
    (- STAGE-WIDTH 200)
    (- STAGE-HEIGHT 200)
    MTScene
  )
)

;; Stub:
;; (define (render bloom) MTScene)

;; Template from BalloonState
;; * Compound: 6 fields
(define (render bloom)
  (place-image
    (circle (bloom-r bloom) "solid" (bloom-colour bloom))
    (bloom-x bloom)
    (bloom-y bloom)
    MTScene
  )
)
```

### Module 4a: Self-Reference

#### List Mechanism

* `cons <element> empty`
* `first`

* Simplest type of arbitrary-size data is a list of values
* I really want to make myself feel that there is some deeper meaning behind it, and I'm sure there is always someone who can argue with references to computer science fundamentals, but lists in BSL looks like insanity so far. I'm not sure how `(cons 1 (cons 2 (cons 3 empty)))` is easier to understand for a beginner than `[1, 2, 3]`. It may also just be me being ignorant, of course.
* Ohhhhhh, the self-referencing seems to be related to how iterators work and it relates back to the whole point of being arbitrary-size. I'm an idiot.

#### Discussion Topic

> The self-reference in the ListOfNatural type comment means that the type describes arbitrary-sized data. The list can have 0 elements, 1, 2 … any number. The idea that something can refer to itself may be surprising to you. Have you seen anything like self reference before? Where? (If you have programmed before, please do not use loops of any kind from other languages.)

Recursion.

#### Function Operating on List

* Ahhhhhh, recursion, there it is

#### Revising the Recipe for Lists

* A well-formed self-reference will always have at least one base, non-self-reference case (`empty` in list, for example) and at least one self-reference case.

#### Discussion Topic

> You have now seen that because of the way the ListOfNumber type comment uses a base case, a cons and a self-reference it can represent arbitrary sized data. The type comment also leads to a template with a natural recursion. That template has 3 positions we can fill in, the base-case result, the contribution of the first and the combination. What the table suggests is that a given function might be specified from the type comment and the value that fills the positions alone.  Can you imagine programming that way?

Yup! Everything is now coming together a lot more clearly and have learnt a lot!

#### Recommended Problems

**Self-Reference P2 - Double All**

Attempt:

```BSL
;; ListOfNumber -> ListOfNumber
;; For every number m in the list, return m * 2

(check-expect (double-all empty) empty)
(check-expect (double-all (cons 1 empty)) (cons 2 empty))
(check-expect (double-all (cons 2 (cons 1 empty))) (cons 4 (cons 2 empty)))

;; Stub:
;; (define (double-all list) empty)

;; Using template from ListOfNumber definition
(define (double-all lon)
  (cond [(empty? lon) empty]
        [else
         (cons (* (first lon) 2)
               (double-all (rest lon)))]))
```

**Self-Reference P3 - Boolean List**

Attempts:

```BSL
;; =================
;; Data definitions:
;; =================

; PROBLEM A:
;
; Design a data definition to represent a list of booleans. Call it ListOfBoolean.


;; ListOfBoolean is one of:
;; * empty
;; * (cons Boolean ListOfBoolean)
;; interp. a list of Booleans

(define LOB1 empty);
(define LOB2 (cons true empty));
(define LOB3 (cons false empty));
(define LOB4 (cons true (cons true empty)));
(define LOB5 (cons false (cons true empty)));
(define LOB6 (cons true (cons false empty)));
(define LOB7 (cons false (cons false empty)));

;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: empty
;; * Compound: (cons Boolean ListOfBoolean)
;; * Self-reference: (rest lob) is ListOfBoolean

(define (fn-for-lob lob)
  (cond [(empty? lob) (...)]
        [else (... (first lob)
                   (fn-for-lob (rest lob)))]))

;; =================
;; Functions:
;; =================

; PROBLEM B:
;
; Design a function that consumes a list of boolean values and produces true
; if every value in the list is true. If the list is empty, your function
; should also produce true. Call it all-true?

;; ListOfBoolean -> Boolean
;; Check if every item in the last is true; if yes, return true; otherwise, return false.

(check-expect (all-true empty) false)
(check-expect (all-true (cons true empty)) true)
(check-expect (all-true (cons false empty)) false)
(check-expect (all-true (cons true (cons true empty))) true)
(check-expect (all-true (cons false (cons true empty))) false)
(check-expect (all-true (cons true (cons false empty))) false)
(check-expect (all-true (cons false (cons false empty))) false)

;; Stub:
;; (define (all-true lob) false)

;; Using template from ListOfBoolean definition

(define (all-true lob)
  (cond [(empty? lob) false]
        [else (and (first lob)
                   (if (empty? (rest lob))
                       true
                       (all-true (rest lob))))]))
```

**Self-Reference P5 - Largest**

Attempt:

```BSL
;; ListOfNumber -> Natural
;; Find and return the largest number in a list of type ListOfNumber

(check-expect (largest-num empty) 0)
(check-expect (largest-num (cons 0 empty)) 0)
(check-expect (largest-num (cons 7 empty)) 7)
(check-expect (largest-num (cons 42 (cons 7 empty))) 42)

;; Stub
;; (define (largest-num lon) 0)

;; Using template from ListOfNumber definition

(define (largest-num lon)
  (cond [(empty? lon) 0]
        [else
         (max (first lon)
              (largest-num (rest lon)))]))
```

**Self-Reference P6 - Image List**

Attempt:

```BSL
(require 2htdp/image)

;; =================
;; Data definitions:
;; =================

; PROBLEM A:
;
; Design a data definition to represent a list of images. Call it ListOfImage.


;; ListOfImage is one of:
;; * empty
;; * (cons Image ListOfImage)
;; interp. a list of images

(define LOI1 empty)
(define LOI2 (cons (square 42 "solid" "black") (cons (square 24 "solid" "white") empty)))

;; Template rules used:
;;  * One of: 2 cases
;;  * Atomic distinct: empty
;;  * Compound: (cons Image ListOfImage)
;;  * Self-reference: (rest lon) is ListOfImage

(define (fn-for-loi loi)
  (cond [(empty? loi) (...)]
        [else (... (first loi) (fn-for-loi (rest loi)))]))

;; =================
;; Functions:
;; =================

; PROBLEM B:
;
; Design a function that consumes a list of images and produces a number
; that is the sum of the areas of each image. For area, just use the image's
; width times its height.


;; ListOfImage -> Natural
;; Return the sum of the area, (* (image-width Image) (image-height Image)), of each imagein ListOfImage

(check-expect (sigma-area empty) 0)
(check-expect (sigma-area (cons (square 10 "solid" "black") empty)) 100)
(check-expect (sigma-area (cons (square 20 "solid" "black") (cons (square 10 "solid" "black") empty))) 500)

;; Stub:
;; (define (sigma-area loi) 0)

;; Using template from ListOfImage definition

(define (sigma-area loi)
  (cond [(empty? loi) 0]
        [else (+
               (* (image-width (first loi)) (image-height (first loi)))
               (sigma-area (rest loi)))]))
```
