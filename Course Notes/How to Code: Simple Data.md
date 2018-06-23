# How to Code: Simple Data

## Table of Contents

* [How to Code: Simple Data](#how-to-code-simple-data)
  * [Notes](#notes)
  * [Resources](#resources)

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

### 4b: Reference

**Reference P1 - Alternative Tuition Graph**

Attempt:

```BSL

;; alternative-tuition-graph-starter.rkt

;
; Consider the following alternative type comment for Eva's school tuition
; information program. Note that this is just a single type, with no reference,
; but it captures all the same information as the two types solution in the
; videos.
;
; (define-struct school (name tuition next))
; ;; School is one of:
; ;;  - false
; ;;  - (make-school String Natural School)
; ;; interp. an arbitrary number of schools, where for each school we have its
; ;;         name and its tuition in USD
;
; (A) Confirm for yourself that this is a well-formed self-referential data
;     definition.
;
; (B) Complete the data definition making sure to define all the same examples as
;     for ListOfSchool in the videos.
;
; (C) Design the chart function that consumes School. Save yourself time by
;     simply copying the tests over from the original version of chart.
;
; (D) Compare the two versions of chart. Which do you prefer? Why?
;


;; ===========
;; Problem (A)
;; ===========

;; The type comment is self-referential because School refers back to
;; itself in (make-school String Natural School). When no school is present
;; it defaults to false.

;; ===========
;; Problem (B)
;; ===========

(define-struct school (name tuition next))
;; School is one of:
;;  - false
;;  - (make-school String Natural School)
;; interp. an arbitrary number of schools, where for each school we have its
;;         name and its tuition in USD
(define S1 false)
(define S2 (make-school "School1" 27797
                        (make-school "School2" 23300
                                     (make-school "School3" 28500 false))))
;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: false
;; * Compound: (make-school String Natural School)
;; * Self reference: (fn-for-school (school-next los))
#;
(define (fn-for-school los)
  (cond [(false?) (...)]
        [else
         (... (school-name los)
         (school-tuition los)
         (fn-for-school (school-next los)))]))

;; ===========
;; Problem (C)
;; ===========

(require 2htdp/image)

;; Constants:
(define FONT-SIZE 24)
(define FONT-COLOR "black")

(define Y-SCALE   1/200)
(define BAR-WIDTH 30)
(define BAR-COLOR "lightblue")

;; School -> Image
;; Produce a bar chat for a list of school, where the height of each bar is proportional to its tuition fee
(check-expect (chart false) (square 0 "solid" "white"))
(check-expect (chart (make-school "S1" 8000 false))
              (beside/align "bottom"
                            (overlay/align "center" "bottom"
                                           (rotate 90 (text "S1" FONT-SIZE FONT-COLOR))
                                           (rectangle BAR-WIDTH (* 8000 Y-SCALE) "outline" "black")
                                           (rectangle BAR-WIDTH (* 8000 Y-SCALE) "solid" BAR-COLOR))
                            (square 0 "solid" "white")))

(check-expect (chart (make-school "S2" 12000 (make-school "S1" 8000 false)))
              (beside/align "bottom"
                            (overlay/align "center" "bottom"
                                           (rotate 90 (text "S2" FONT-SIZE FONT-COLOR))
                                           (rectangle BAR-WIDTH (* 12000 Y-SCALE) "outline" "black")
                                           (rectangle BAR-WIDTH (* 12000 Y-SCALE) "solid" BAR-COLOR))
                            (overlay/align "center" "bottom"
                                           (rotate 90 (text "S1" FONT-SIZE FONT-COLOR))
                                           (rectangle BAR-WIDTH (* 8000 Y-SCALE) "outline" "black")
                                           (rectangle BAR-WIDTH (* 8000 Y-SCALE) "solid" BAR-COLOR))
                            (square 0 "solid" "white")))

;; Stub:
;; (define (chart los) (square 0 "solid" "white"));

;; Template:
;; Use template from ListOfSchool definition:

(define (chart los)
  (cond [(false? los) (square 0 "solid" "white")]
        [else
         (beside/align "bottom"
                       (overlay/align "center" "bottom"
                                           (rotate 90 (text (school-name los) FONT-SIZE FONT-COLOR))
                                           (rectangle BAR-WIDTH (* (school-tuition los) Y-SCALE) "outline" "black")
                                           (rectangle BAR-WIDTH (* (school-tuition los) Y-SCALE) "solid" BAR-COLOR))
                       (chart (school-next los)))]))

;; ===========
;; Problem (D)
;; ===========

;; Without breaking up los, and therefore the data structure, it is not possible
;; to write a make-bar function as it was done using list in the videos (which has
;; the `first` and `rest` syntax available for iteration. Therefore the one
;; presented in the video is preferred.
```

**Reference P2 - Spinning Bears**

Skipped in favour of moving forward.

### 5a: Naturals

**Naturals P2 - Decreasing Image**

Attempt:

```BSL
(require 2htdp/image)

;; decreasing-image-starter.rkt

;  PROBLEM:
;  
;  Design a function called decreasing-image that consumes a Natural n and produces an image of all the numbers
;  from n to 0 side by side.
;  
;  So (decreasing-image 3) should produce .


;; Natural is one of:
;; * 0
;; * (add1 Natural)
;; Interp. a natural number

(define N0 0)
(define N1 (add1 N0))
(define N2 (add1 N1))

#;
(define (fn-for-natural n)
  (cond [(zero? n)(...)]
        [else
         (... n
              (fn-for-natural (sub1 n)))]))

;; Natural -> Image
;; Produce an images of natural numbers for n, n - 1, n - 2, ..., 0
(check-expect (decreasing-image 0) (text "0" 24 "black"))
(check-expect (decreasing-image 1) (beside (text "1" 24 "black")
                                           (text "0" 24 "black")))
(check-expect (decreasing-image 3) (beside (text "3" 24 "black")
                                           (text "2" 24 "black")
                                           (text "1" 24 "black")
                                           (text "0" 24 "black")))

;; Stub:
;; (define (decreasing-image n) (text "0" 24 "black"))

;; Using template for Natural:

(define (decreasing-image n)
  (cond [(zero? n) (text "0" 24 "black")]
        [else
         (beside (text (number->string n) 24 "black")
                 (decreasing-image (sub1 n)))]))
```

**Naturals P2 - Decreasing Image**

```BSL
;; odd-from-n-starter.rkt

;  PROBLEM:
;  
;  Design a function called odd-from-n that consumes a natural number n, and produces a list of all
;  the odd numbers from n down to 1.
;  
;  Note that there is a primitive function, odd?, that produces true if a natural number is odd.

; Natural is one of:
;; * 0
;; * (add1 Natural)
;; Interp. a natural number

(define N0 0)
(define N1 (add1 N0))
(define N2 (add1 N1))

#;
(define (fn-for-natural n)
  (cond [(zero? n)(...)]
        [else
         (... n
              (fn-for-natural (sub1 n)))]))

;; Natural -> ListOfNatural
;; Produce a list of all the odd natural numbers between n and 1, inclusive
(check-expect (odd-from-n 1) (cons 1 empty))
(check-expect (odd-from-n 3) (cons 3 (cons 1 empty)))
(check-expect (odd-from-n 5) (cons 5 (cons 3 (cons 1 empty))))

;; Stub:
;; (define (odd-from-n n) (cons 1 empty))

;; Using template from Natural
(define (odd-from-n n)
  (cond [(= n 1) (cons 1 empty)]
        [else (if (odd? n)
                  (cons n
                        (odd-from-n (sub1 n)))
                  (odd-from-n (sub1 n)))]))
```

### Module 5b: Helpers

#### Function Composition, pt 1

> (A) Design a data definition to represent an arbitrary umber of images

```BSL
(require 2htdp/image)

;; ListOfImages is one of:
;; * empty
;; * (cons Image ListOfImages)

(define LOI1 empty)
(define LOI2 (cons (square 0 "solid" "white") empty))

;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: empty
;; * Compound: (cons Image ListOfImages)
;;   * Atomic non-distinct: Image
;;   * Self-reference: (rest ListOfImages)
#;
(define (fn-for-loi loi)
  (cond [(empty? empty) (...)]
        [else (... (first loi)
                   (fn-for-loi (rest loi)))]))
```

#### Domain Knowledge Shift

* When knowledge domain shifts, a new function is required

#### Quiz

Attempt:

```BSL
(require 2htdp/image)

;; ======================================================================
;; Constants
(define COOKIES .)

;; ======================================================================
;; Data Definitions

;; Natural is one of:
;;  - 0
;;  - (add1 Natural)
;; interp. a natural number
(define N0 0)         ;0
(define N1 (add1 N0)) ;1
(define N2 (add1 N1)) ;2

#;
(define (fn-for-natural n)
  (cond [(zero? n) (...)]
        [else
         (... n   ; n is added because it's often useful                   
              (fn-for-natural (sub1 n)))]))

;; Template rules used:
;;  - one-of: two cases
;;  - atomic distinct: 0
;;  - compound: 2 fields
;;  - self-reference: (sub1 n) is Natural




; PROBLEM 1:
;
; Complete the design of a function called pyramid that takes a natural
; number n and an image, and constructs an n-tall, n-wide pyramid of
; copies of that image.
;
; For instance, a 3-wide pyramid of cookies would look like this:
;
; .


;; Natural Image -> Image
;; produce an n-wide pyramid of the given image
(check-expect (pyramid 0 COOKIES) empty-image)
(check-expect (pyramid 1 COOKIES) COOKIES)
(check-expect (pyramid 3 COOKIES)
              (above COOKIES
                     (beside COOKIES COOKIES)
                     (beside COOKIES COOKIES COOKIES)))

;; (define (pyramid n img) empty-image) ; stub

;; Using template for Natural
;; Swapped order of n and (pyramid (sub1 n)) expression to get the right order

(define (pyramid n img)
  (cond [(zero? n) empty-image]
        [else
         (above (pyramid (sub1 n) img)
                (line-up-cookies n img))]))

;; Natural Image -> ListOfCookieImages
;; Line up the appropriate amount of cookies for rendering
(check-expect (line-up-cookies 0 COOKIES) empty-image)
(check-expect (line-up-cookies 1 COOKIES) COOKIES)
(check-expect (line-up-cookies 2 COOKIES) (beside COOKIES COOKIES))

;; Stub:
;; (define (line-up-cookies n img) empty-image)

;; Using template for Natural:

(define (line-up-cookies n img)
  (cond [(zero? n) empty-image]
        [else
         (beside img
                 (line-up-cookies (sub1 n) img))]))


; Problem 2:
; Consider a test tube filled with solid blobs and bubbles.  Over time the
; solids sink to the bottom of the test tube, and as a consequence the bubbles
; percolate to the top.  Let's capture this idea in BSL.
;
; Complete the design of a function that takes a list of blobs and sinks each
; solid blob by one. It's okay to assume that a solid blob sinks past any
; neighbor just below it.
;
; To assist you, we supply the relevant data definitions.


;; Blob is one of:
;; - "solid"
;; - "bubble"
;; interp.  a gelatinous blob, either a solid or a bubble
;; Examples are redundant for enumerations
#;
(define (fn-for-blob b)
  (cond [(string=? b "solid") (...)]
        [(string=? b "bubble") (...)]))

;; Template rules used:
;; - one-of: 2 cases
;; - atomic distinct: "solid"
;; - atomic distinct: "bubble"




;; ListOfBlob is one of:
;; - empty
;; - (cons Blob ListOfBlob)
;; interp. a sequence of blobs in a test tube, listed from top to bottom.
(define LOB0 empty) ; empty test tube
(define LOB2 (cons "solid" (cons "bubble" empty))) ; solid blob above a bubble

#;
(define (fn-for-lob lob)
  (cond [(empty? lob) (...)]
        [else
         (... (fn-for-blob (first lob))
              (fn-for-lob (rest lob)))]))

;; Template rules used
;; - one-of: 2 cases
;; - atomic distinct: empty
;; - compound: 2 fields
;; - reference: (first lob) is Blob
;; - self-reference: (rest lob) is ListOfBlob

;; ListOfBlob -> ListOfBlob
;; produce a list of blobs that sinks the given solid blobs by one

(check-expect (sink empty) empty)
#;(check-expect (sink (cons "bubble" (cons "solid" (cons "bubble" empty))))
              (cons "bubble" (cons "bubble" (cons "solid" empty))))
(check-expect (sink (cons "solid" (cons "solid" (cons "bubble" empty))))
              (cons "bubble" (cons "solid" (cons "solid" empty))))
(check-expect (sink (cons "solid" (cons "bubble" (cons "bubble" empty))))
              (cons "bubble" (cons "solid" (cons "bubble" empty))))
(check-expect (sink (cons "solid" (cons "bubble" (cons "solid" empty))))
              (cons "bubble" (cons "solid" (cons "solid" empty))))
(check-expect (sink (cons "bubble" (cons "solid" (cons "solid" empty))))
              (cons "bubble" (cons "solid" (cons "solid" empty))))
(check-expect (sink (cons "solid"
                          (cons "solid"
                                (cons "bubble" (cons "bubble" empty)))))
              (cons "bubble" (cons "solid"
                                   (cons "solid" (cons "bubble" empty)))))

;; Stub:
;; (define (sink lob) empty)

;; Using native helper functions to reverse a list for sorting
(define (sink lob)
  (reverse (sink-reversed (reverse lob)))
)

;; ListOfBlob -> ListOfBlob
;; For every item in ListOfBubble, swap its position with the next item on the list
;; if, and only if, the current item is a bubble
(check-expect (sink-reversed empty) empty)
(check-expect (sink-reversed (cons "solid" empty))
              (cons "solid" empty))
(check-expect (sink-reversed (cons "bubble" (cons "solid" empty)))
              (cons "solid" (cons "bubble" empty)))
(check-expect (sink-reversed (cons "bubble" (cons "bubble" (cons "solid" empty))))
              (cons "bubble" (cons "solid" (cons "bubble" empty))))
(check-expect (sink-reversed (cons "bubble" (cons "bubble" (cons "solid" (cons "solid" empty)))))
              (cons "bubble" (cons "solid" (cons "solid" (cons "bubble" empty)))))

;; Stub
;; (define (sink-reversed lob) empty)

;; Using template for Natural

(define (sink-reversed lob)
  (cond [(empty? lob) empty]
        [(empty? (rest lob)) lob]
        [else (if (string=? (first lob) "bubble")
                  (cons (first (rest lob)) (sink-reversed (cons (first lob) (rest (rest lob)))))
                  (cons (first lob) (sink-reversed (rest lob)))
        )]))
```

**Remarks after watching evaluation video**

Solution to problem B definitely deviated from the templates and is not ideal. Reversing the list
was a bad idea to begin with since the problem can (now obviously) be solved without reversing
the list. Should have taken a break and come back to it instead of pressing through it.

### Module 6: Binary Search Tree

#### Finish Problem from Starter

Atttempt:

```BSL
;; CONSTANTS

(define TEST-ACCOUNTS (list (make-account 1 "abc") (make-account 4 "dcj") (make-account 3 "ilk")   (make-account 7 "ruf")))

;; Accounts Natural -> String or false
;; Try to find account with given number in accounts. If found produce name, otherwise produce false.
(check-expect (lookup-name empty 1) false)
(check-expect (lookup-name TEST-ACCOUNTS 1) "abc")
(check-expect (lookup-name TEST-ACCOUNTS 7) "ruf")
(check-expect (lookup-name TEST-ACCOUNTS 42) false)

;; Stub
;; (define (lookup-name accs n) false)

;; Using template for Accounts
(define (lookup-name accs n)
  (cond [(empty? accs) false]
        [else
         (if (= (account-num (first accs)) n)
             (account-name (first accs))
             (lookup-name (rest accs) n))]))
```

#### Recommend Problems

**BST P2 - Sum Keys**

```BSL
;; BST -> Natural
;; Produce the sum of all keys in the BST
(check-expect (sum-of-keys BST0) 0)
(check-expect (sum-of-keys BST1) 1)
(check-expect (sum-of-keys BST4) 11)
(check-expect (sum-of-keys BST3) 15)
(check-expect (sum-of-keys BST42) 83)
(check-expect (sum-of-keys BST10) 108)

;; Stub:
;; (define (sum-of-keys BST0) 0)

;; Using template from BST definition:

(define (sum-of-keys t)
  (cond [(false? t) 0]
        [else
         (+ (node-key t)
            (sum-of-keys (node-l t))
            (sum-of-keys (node-r t)))]))
```

**BST P4 - Insert**

```BSL
;; Integer String BST -> BST
;; Insert the key: value pair in the correct position in the BST accoriding
;; to the key given
(check-expect (insert-node 3 "ilk" BST0)
              (make-node 3 "ilk" false false))
(check-expect (insert-node 1 "abc" (make-node 3 "ilk" false false))
              (make-node 3 "ilk" (make-node 1 "abc" false false) false))
(check-expect (insert-node 4 "dcj" (make-node 3 "ilk" (make-node 1 "abc" false false) false))
              (make-node 3 "ilk" (make-node 1 "abc" false false) (make-node 4 "dcj" false false)))
(check-expect (insert-node 2 "nya" (make-node 3 "ilk" (make-node 1 "abc" false false) (make-node 4 "dcj" false false)))
              (make-node 3 "ilk" (make-node 1 "abc" false (make-node 2 "nya" false false)) (make-node 4 "dcj" false false)))

;; (define (insert-node key value bst) BST0)

;; Using template for BST
(define (insert-node key value t)
  (cond [(false? t) (make-node key value false false)]
        [else
         (cond [(> (node-key t) key)
                (make-node
                 (node-key t)
                 (node-val t)
                 (insert-node key value (node-l t))
                 (node-r t))]
               [(< (node-key t) key)
                (make-node
                 (node-key t)
                 (node-val t)
                 (node-l t)
                 (insert-node key value (node-r t)))])]))
```

### Final Project

```BSL
(require 2htdp/universe)
(require 2htdp/image)

;; =================
;; Space Invaders
;; =================

;; =================
;; Constants:
;; =================

(define WIDTH  300)
(define HEIGHT 500)

(define INVADER-X-SPEED 2)  ;speeds (not velocities) in pixels per tick
(define INVADER-Y-SPEED 2)
(define TANK-SPEED 5)
(define MISSILE-SPEED 15)

(define HIT-RANGE 10)

(define INVADE-RATE 100)

(define BACKGROUND (empty-scene WIDTH HEIGHT))

; (overlay/xy (ellipse 10 15 "outline" "blue")              ;cockpit cover
;               -5 6
;               (ellipse 20 10 "solid"   "blue"))

(define INVADER .) ;; Replace . with image
(define INVADER-WIDTH (image-width INVADER))
(define INVADER-HEIGHT (image-height INVADER))


(define TANK
  (overlay/xy (overlay (ellipse 28 8 "solid" "black")       ;tread center
                       (ellipse 30 10 "solid" "green"))     ;tread outline
              5 -14
              (above (rectangle 5 10 "solid" "black")       ;gun
                     (rectangle 20 10 "solid" "black"))))   ;main body

(define TANK-HEIGHT (image-height TANK))
(define TANK-HEIGHT/2 (/ TANK-HEIGHT 2))
(define TANK-Y (- HEIGHT TANK-HEIGHT))

(define MISSILE (circle 5 "solid" "red"))
(define MISSILE-HEIGHT (image-height MISSILE))

(define SPAWN-RATE 40)

;; =================
;; Data Definitions:
;; =================

(define-struct game (invaders missiles tank))
;; Game is (make-game  (listof Invader) (listof Missile) Tank)
;; interp. the current state of a space invaders game
;;         with the current invaders, missiles and tank position

;; Game constants defined below Missile data definition

#;
(define (fn-for-game s)
  (... (fn-for-loinvader (game-invaders s))
       (fn-for-lom (game-missiles s))
       (fn-for-tank (game-tank s))))



(define-struct tank (x dir))
;; Tank is (make-tank Number Integer[-1, 1])
;; interp. the tank location is x, HEIGHT - TANK-HEIGHT/2 in screen coordinates
;;         the tank moves TANK-SPEED pixels per clock tick left if dir -1, right if dir 1

(define T0 (make-tank (/ WIDTH 2) 0))   ;center going right
(define T1 (make-tank 50 1))            ;going right
(define T2 (make-tank 50 -1))           ;going left

#;
(define (fn-for-tank t)
  (... (tank-x t) (tank-dir t)))



(define-struct invader (x y dx))
;; Invader is (make-invader Number Number Number)
;; interp. the invader is at (x, y) in screen coordinates
;;         the invader along x by dx pixels per clock tick

(define I1 (make-invader 150 100 12))           ;not landed, moving right
(define I2 (make-invader 150 HEIGHT -10))       ;exactly landed, moving left
(define I3 (make-invader 150 (+ HEIGHT 10) 10)) ;> landed, moving right


#;
(define (fn-for-invader invader)
  (... (invader-x invader) (invader-y invader) (invader-dx invader)))


(define-struct missile (x y))
;; Missile is (make-missile Number Number)
;; interp. the missile's location is x y in screen coordinates

(define M1 (make-missile 150 300))                       ;not hit U1
(define M2 (make-missile (invader-x I1) (+ (invader-y I1) 10)))  ;exactly hit U1
(define M3 (make-missile (invader-x I1) (+ (invader-y I1)  5)))  ;> hit U1

#;
(define (fn-for-missile m)
  (... (missile-x m) (missile-y m)))



(define G0 (make-game empty empty T0))
(define G1 (make-game empty empty T1))
(define G2 (make-game (list I1) (list M1) T1))
(define G3 (make-game (list I1 I2) (list M1 M2) T1))

;; ListOfInvaders is one of:
;; * empty
;; * (cons Invader ListOfInvaders)

(define LOI0 empty)
(define LOI1 (cons I1 empty))
(define LOI2 (cons I2 (cons I1 empty)))

;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: empty
;; * Compound: (cons Invader ListOfInvaders)
;; * Self-reference: (rest loi) is ListOfInvaders

#;
(define (fn-for-loi loi)
  (cond [(empty? loi) (...)]
        [else (... (fn-for-invader (first loi))
                   (fn-for-loi (rest loi)))]))

;; ListOfMissiles is one of:
;; * empty
;; * (cons Missile ListOfMissiles)

(define LOM0 empty)
(define LOM1 (cons M1 empty))
(define LOM2 (cons M2 (cons M1 empty)))

;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: empty
;; * Compound: (cons Missile ListOfMissiles)
;; * Self-reference: (rest lom) is ListOfMissiles

#;
(define (fn-for-lom lom)
  (cond [(empty? lom) (...)]
        [else (... (fn-for-missile (first lom))
                   (fn-for-lom (rest lom)))]))

;; ListOfImages is one of:
;; * empty
;; * (cons Image ListOfImages)

(define LOIMG0 empty-image)
(define LOIMG1 (list TANK))
(define LOIMG2 (list INVADER TANK))

;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: empty
;; * Compound: (cons Image ListOfImages)
;; * Self-reference: (rest loimg) is ListOfImages

(define (fn-for-loimg loimg)
  (cond [(empty? loimg) (...)]
        [else (... (first loimg)
                   (fn-for-loimg (rest loimg)))]))

;; ListOfPosns is one of:
;; * empty
;; * (cons Posn ListOfPosns)

(define POSNS0 empty)
(define POSNS1 (list (make-posn (tank-x T0) TANK-Y)))
(define POSNS2 (list (make-posn (tank-x T0) TANK-Y) (make-posn (invader-x I1) (invader-y I1))))

;; Template rules:
;; * One of: 2 cases
;; * Atomic distinct: empty
;; * Compound: (cons Posn ListOfPosns)
;; * Self-reference: (rest loposn) is ListOfPosns

(define (fn-for-loposn loposn)
  (cond [(empty? loposn) (...)]
        [else (... (first loposn)
                   (fn-for-loposn (rest loposn)))]))

;; =================
;; Test Constants:
;; =================

(define INVADER-IMG-WIDTH (image-width INVADER))
(define INVADER-MID-LEFT (make-invader 150 100 -1))
(define INVADER-MID-LEFT-NEXT (make-invader (- 150 INVADER-X-SPEED) (+ 100 INVADER-Y-SPEED) -1))
(define INVADER-MID-RIGHT (make-invader 150 100 1))
(define INVADER-MID-RIGHT-NEXT (make-invader (+ 150 INVADER-X-SPEED) (+ 100 INVADER-Y-SPEED) 1))
(define INVADER-LEFTBOUNCE (make-invader INVADER-X-SPEED 100 -1))
(define INVADER-LEFTBOUNCE-NEXT (make-invader 0 (+ 100 INVADER-Y-SPEED) 1))
(define INVADER-RIGHTBOUNCE (make-invader (- (- WIDTH INVADER-IMG-WIDTH) INVADER-X-SPEED) 100 1))
(define INVADER-RIGHTBOUNCE-NEXT (make-invader (- WIDTH INVADER-IMG-WIDTH) (+ 100 INVADER-Y-SPEED) -1))

(define MISSILE-FREE (make-missile 150 300))
(define MISSILE-FREE-NEXT (make-missile 150 (- (missile-y MISSILE-FREE) MISSILE-SPEED)))
(define MISSILE-FREE2 (make-missile 80 180))
(define MISSILE-HIT (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))
(define MISSILE-HIT-NEXT (make-missile (invader-x INVADER-MID-LEFT) (- (invader-y INVADER-MID-LEFT) MISSILE-SPEED)))
(define MISSILE-OUB (make-missile 150 (- 0 MISSILE-HEIGHT)))

;; =================
;; Functions:
;; =================

;; Game -> Game
;; Shortcut for (main (make-game empty empty T0))
;; Start the game with (init 0)
(define (init n)
  (main (make-game empty empty T0)))

;; =================
;; main
;; =================

;; Game -> Game
;; start the world with (main (make-game empty empty T0)) or simply (init 0)
(define (main game)
  (big-bang game
    (on-tick update-game) ;; Game -> Game
    (to-draw render-game) ;; Game -> Image
    (on-key handle-key) ;; Game -> Game
    (on-release handle-release) ;; Game -> Game
    (stop-when gameover-check) ;; Game -> Boolean
  )
)


;; =================
;; update-game
;; =================

;; Game -> Game
;; Produce the next game state

;; Stub:
;; (define (update-game game) (make-game empty empty T0))

;; Using template for Game:
(define (update-game game)
   (randomly-generate-invader (collision-detection (update-pos game))))


;; =========================
;; randomly-generate-invader
;; =========================

;; Game -> Game
;; Randomly generate invaders

;; Stub:
;; (define (randomly-generate-invader game) game)

;; Using template from Game:

(define (randomly-generate-invader game)
  (make-game (cond [(< (random SPAWN-RATE) 1) (spawn-invader (game-invaders game))]
                   [else (game-invaders game)])
             (game-missiles game)
             (game-tank game)))


;; =============
;; spawn-invader
;; =============

;; ListOfInvaders -> ListOfInvaders
;; Spawn an invader
(check-expect (length (spawn-invader empty)) 1)
(check-expect (length (spawn-invader (list INVADER-MID-LEFT))) 2)

;; Stub:
;; (define (spawn-invader game) game)

;; Using template from ListOfInvaders:

(define (spawn-invader loi)
  (cons (make-invader (+ INVADER-WIDTH (random (- WIDTH (* INVADER-WIDTH 2))))
                      (- 0 INVADER-HEIGHT)
                      (if (> (random 2) 0)
                          1
                          -1))
        loi))


;; =================
;; update-pos
;; =================

;; Game -> Game
;; Update positions of invaders and missiles
(check-expect (update-game (make-game empty empty T0)) (make-game empty empty T0))
(check-expect (update-game (make-game (list INVADER-MID-LEFT) empty T0))
              (make-game (list INVADER-MID-LEFT-NEXT) empty T0))
(check-expect (update-game (make-game (list INVADER-MID-LEFT INVADER-MID-RIGHT) empty T0))
              (make-game (list INVADER-MID-LEFT-NEXT INVADER-MID-RIGHT-NEXT) empty T0))
(check-expect (update-game (make-game empty (list MISSILE-FREE) T0))
              (make-game empty (list MISSILE-FREE-NEXT) T0))
(check-expect (update-game (make-game empty (list MISSILE-FREE MISSILE-HIT) T0))
              (make-game empty (list MISSILE-FREE-NEXT MISSILE-HIT-NEXT) T0))
(check-expect (update-game (make-game (list INVADER-LEFTBOUNCE INVADER-RIGHTBOUNCE) (list MISSILE-FREE MISSILE-HIT) T0))
              (make-game (list INVADER-LEFTBOUNCE-NEXT INVADER-RIGHTBOUNCE-NEXT) (list MISSILE-FREE-NEXT MISSILE-HIT-NEXT) T0))

;; Stub:
;; (define (update-pos game) (make-game empty empty T0))


;; Using template for Game:

(define (update-pos game)
  (make-game (update-loi-pos (game-invaders game))
             (update-lom-pos (game-missiles game))
             (update-tank-pos (game-tank game))))


;; ===============
;; update-tank-pos
;; ===============

;; Tank -> Tank
;; Move the tank according to the current direction
(check-expect (update-tank-pos (make-tank 100 0)) (make-tank 100 0))
(check-expect (update-tank-pos (make-tank 100 1))
              (make-tank (+ 100 TANK-SPEED) 1))
(check-expect (update-tank-pos (make-tank 100 -1))
              (make-tank (+ 100 (* TANK-SPEED -1)) -1))

;; Stub:
;; (define (update-tank-pos tank) T0)

;; Using template for Tank:
(define (update-tank-pos tank)
  (make-tank (+ (tank-x tank) (* TANK-SPEED (tank-dir tank))) (tank-dir tank)))


;; =================
;; update-loi-pos
;; =================

;; ListOfInvaders -> ListOfInvaders
;; Apply physics updates to the list of invaders
(check-expect (update-loi-pos empty) empty)
(check-expect (update-loi-pos (list INVADER-MID-LEFT)) (list INVADER-MID-LEFT-NEXT))
(check-expect (update-loi-pos (list INVADER-MID-LEFT INVADER-MID-RIGHT)) (list INVADER-MID-LEFT-NEXT INVADER-MID-RIGHT-NEXT))

;; Stub
;; (define (update-loi-pos loi) empty)

;; Using template for ListOfInvaders:
(define (update-loi-pos loi)
  (cond [(empty? loi) empty]
        [else (cons (update-invader (first loi))
                    (update-loi-pos (rest loi)))]))


;; =================
;; update-invader
;; =================

;; Invader -> Invader
;; Apply physics update to a single invader
(check-expect (update-invader INVADER-MID-LEFT) INVADER-MID-LEFT-NEXT)
(check-expect (update-invader INVADER-MID-RIGHT) INVADER-MID-RIGHT-NEXT)
(check-expect (update-invader INVADER-LEFTBOUNCE) INVADER-LEFTBOUNCE-NEXT)
(check-expect (update-invader INVADER-RIGHTBOUNCE) INVADER-RIGHTBOUNCE-NEXT)

;; Stub:
;; (define (update-invader invader) (make-invader 150 100 1))

;; Using template for Invader:
(define (update-invader invader)
  (update-invader-dx (update-invader-pos invader)))


;; ==================
;; update-invader-pos
;; ==================

;; Invader -> Invader
;; Update x and y positions of invader
(check-expect (update-invader-pos INVADER-MID-LEFT) INVADER-MID-LEFT-NEXT)
(check-expect (update-invader-pos INVADER-MID-RIGHT) INVADER-MID-RIGHT-NEXT)
(check-expect (update-invader-pos INVADER-LEFTBOUNCE) (make-invader 0 (+ 100 INVADER-Y-SPEED) -1))
(check-expect (update-invader-pos INVADER-RIGHTBOUNCE) (make-invader (- WIDTH INVADER-IMG-WIDTH) (+ 100 INVADER-Y-SPEED) 1))

;; Stub:
;; (define (update-invader-pos invader) INVADER-MID-LEFT)

;; Using template from Invader:
(define (update-invader-pos invader)
  (make-invader (+ (invader-x invader) (* (invader-dx invader) INVADER-X-SPEED))
                (+ (invader-y invader) INVADER-Y-SPEED)
                (invader-dx invader)
  )
)


;; =================
;; update-invader-dx
;; =================

;; Invader -> Invader
;; Update dx position of invader
(check-expect (update-invader-dx (make-invader 150 100 -1)) (make-invader 150 100 -1))
(check-expect (update-invader-dx (make-invader 150 100 1)) (make-invader 150 100 1))
(check-expect (update-invader-dx (make-invader 0 100 -1)) (make-invader 0 100 1))
(check-expect (update-invader-dx (make-invader (- WIDTH INVADER-IMG-WIDTH) 100 1)) (make-invader (- WIDTH INVADER-IMG-WIDTH) 100 -1))

;; Stub:
;; (define (update-invader-pos invader) INVADER-MID-LEFT)

;; Using template from Invader:
(define (update-invader-dx invader)
  (make-invader (invader-x invader)
                (invader-y invader)
                (if (or (<= (invader-x invader) 0)
                        (>= (invader-x invader) (- WIDTH INVADER-IMG-WIDTH)))
                    (* (invader-dx invader) -1)
                    (invader-dx invader))
  )
)


;; ===================
;; collision-detection
;; ===================

;; Game -> Game
;; Execute collision detection algorithm and remove the following:
;; * Out-of-bounds missiles
;; * Missiles collided with an invaders
;; * Invaders collided with missiles

;; List of invaders examples
(check-expect (collision-detection (make-game empty empty T0)) (make-game empty empty T0))
(check-expect (collision-detection (make-game (list INVADER-MID-LEFT) empty T0)) (make-game (list INVADER-MID-LEFT) empty T0))
(check-expect (collision-detection (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty T0)) (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty T0))
(check-expect (collision-detection (make-game empty (list MISSILE-FREE) T0)) (make-game empty (list MISSILE-FREE) T0))
(check-expect (collision-detection (make-game empty (list MISSILE-FREE MISSILE-HIT) T0)) (make-game empty (list MISSILE-FREE MISSILE-HIT) T0))
(check-expect (collision-detection (make-game empty (list MISSILE-OUB) T0)) (make-game empty empty T0))
(check-expect (collision-detection (make-game empty (list MISSILE-FREE MISSILE-OUB) T0)) (make-game empty (list MISSILE-FREE) T0))
(check-expect (collision-detection (make-game (list INVADER-MID-LEFT) (list MISSILE-OUB) T0)) (make-game (list INVADER-MID-LEFT) empty T0))
(check-expect (collision-detection (make-game (list INVADER-MID-LEFT)
                                              (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))
                                              T0))
              (make-game empty empty T0))
(check-expect (collision-detection (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE)
                                              (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                                                    (make-missile (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE)))
                                              T0))
              (make-game empty empty T0))
(check-expect (collision-detection (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE INVADER-RIGHTBOUNCE)
                                              (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                                                    (make-missile (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE)))
                                              T0))
              (make-game (list INVADER-RIGHTBOUNCE) empty T0))

;; Stub:
;; (define (collision-detection game) (make-game empty empty T0))

;; Using template for Game:
(define (collision-detection game)
  (make-game (collision-detection-invader (game-invaders game) (game-missiles game) empty)
             (collision-detection-missile (game-missiles game) (game-invaders game) empty)
             (game-tank game)))


;; ===========================
;; collision-detection-invader
;; ===========================

;; ListOfInvaders ListOfMissiles ListOfInvaders -> ListOfInvaders
;; For each invader, check if it has collided with any missiles and remove accordingly
(check-expect (collision-detection-invader (list INVADER-MID-LEFT)
                                           (list MISSILE-FREE)
                                           empty)
              (list INVADER-MID-LEFT))
(check-expect (collision-detection-invader (list INVADER-MID-LEFT)
                                           (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))
                                           empty)
              empty)
(check-expect (collision-detection-invader (list INVADER-MID-LEFT)
                                           (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))
                                           empty)
              empty)
(check-expect (collision-detection-invader (list INVADER-MID-LEFT INVADER-LEFTBOUNCE)
                                           (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))
                                           empty)
              (list INVADER-LEFTBOUNCE))
(check-expect (collision-detection-invader (list INVADER-MID-LEFT INVADER-LEFTBOUNCE)
                                           (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                                                 (make-missile (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE)))
                                           empty)
              empty)

;; Stub:
;; (define (collision-detection-invader loi lom newloi) empty)

;; Using template for ListOfInvaders

(define (collision-detection-invader loi lom newloi)
  (cond [(empty? loi) newloi]
        [(empty? lom) loi]
        [else (if (invader-dead? (first loi) lom)
                  (collision-detection-invader (rest loi) lom newloi)
                  (collision-detection-invader (rest loi) lom (append newloi (list (first loi)))))]))


;; =============
;; invader-dead?
;; =============

;; Invader ListOfMissiles -> Boolean
;; For each missile in ListOfMissiles, check if it has collided with the invader
(check-expect (invader-dead? INVADER-MID-LEFT empty) false)
(check-expect (invader-dead? INVADER-MID-LEFT (list (make-missile (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE)))) false)
(check-expect (invader-dead? INVADER-MID-LEFT (list (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))) true)
(check-expect (invader-dead? INVADER-MID-LEFT (list (make-missile (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))
                                                  (make-missile (invader-x INVADER-RIGHTBOUNCE) (invader-y INVADER-RIGHTBOUNCE))))
              false)
(check-expect (invader-dead? INVADER-MID-LEFT (list (make-missile (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))
                                                  (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))))
              true)

;; Stub:
;; (define (invader-dead? invader lom) false)

;; Using template for ListOfMissiles
(define (invader-dead? invader lom)
  (cond [(null? invader) true]
        [(empty? lom) false]
        [else (if (<= (dist-between invader (first lom)) (/ INVADER-HEIGHT 2))
                  (invader-dead? null (rest lom))
                  (invader-dead? invader (rest lom)))]))


;; ===========================
;; collision-detection-missile
;; ===========================

;; ListOfMissiles ListOfInvaders ListOfMissiles -> ListOfMissiles
;; For each missile in ListOfMissiles, check if it has collided with any invaders
;; or the top boundary and remove accordingly
(check-expect (collision-detection-missile (list MISSILE-FREE)
                                           (list INVADER-MID-LEFT)
                                           empty)
              (list MISSILE-FREE))
(check-expect (collision-detection-missile (list MISSILE-FREE MISSILE-HIT)
                                           (list INVADER-LEFTBOUNCE)
                                           empty)
              (list MISSILE-FREE MISSILE-HIT))
(check-expect (collision-detection-missile (list MISSILE-FREE)
                                           (list (make-invader (missile-x MISSILE-FREE) (- (- (missile-y MISSILE-FREE) (/ INVADER-HEIGHT 2)) 1) 1))
                                           empty)
              (list MISSILE-FREE))
(check-expect (collision-detection-missile (list MISSILE-FREE)
                                           (list (make-invader (missile-x MISSILE-FREE) (- (missile-y MISSILE-FREE) (/ INVADER-HEIGHT 2)) 1))
                                           empty)
              empty)
(check-expect (collision-detection-missile (list MISSILE-OUB)
                                           empty
                                           empty)
              empty)
(check-expect (collision-detection-missile (list MISSILE-OUB)
                                           (list INVADER-MID-LEFT)
                                           empty)
              empty)
(check-expect (collision-detection-missile (list MISSILE-FREE)
                                           (list (make-invader (missile-x MISSILE-FREE) (missile-y MISSILE-FREE) 1))
                                           empty)
              empty)
(check-expect (collision-detection-missile (list MISSILE-FREE
                                                 MISSILE-FREE2)
                                           (list (make-invader (missile-x MISSILE-FREE) (missile-y MISSILE-FREE) 1))
                                           empty)
              (list MISSILE-FREE2))
(check-expect (collision-detection-missile (list MISSILE-FREE
                                                 MISSILE-FREE2)
                                           (list (make-invader (missile-x MISSILE-FREE) (missile-y MISSILE-FREE) 1)
                                                 (make-invader (missile-x MISSILE-FREE2) (missile-y MISSILE-FREE2) 1))
                                           empty)
              empty)

;; Stub:
;; (define (collision-detection-missile lom loi newlom) empty)

;; Using template for ListOfInvaders
(define (collision-detection-missile lom loi newlom)
  (cond [(empty? lom) newlom]
        [(<= (missile-y (first lom)) (- 0 MISSILE-HEIGHT))
         (collision-detection-missile (rest lom) loi newlom)] ;; Remove out of bounds missiles
        [else (if (destroy-missile? (first lom) loi)
                  (collision-detection-missile (rest lom) loi newlom)
                  (collision-detection-missile (rest lom) loi (append newlom (list (first lom)))))]))


;; ================
;; destroy-missile?
;; ================

;; Missile ListOfInvaders -> Boolean
;; For each invader in ListOfInvaders, check if it has collided with the invader
(check-expect (destroy-missile? MISSILE-FREE empty) false)
(check-expect (destroy-missile? MISSILE-FREE
                                (list INVADER-MID-LEFT)) false)
(check-expect (destroy-missile? MISSILE-FREE (list (make-invader (missile-x MISSILE-FREE) (missile-y MISSILE-FREE) 1))) true)
(check-expect (destroy-missile? MISSILE-FREE (list INVADER-MID-LEFT INVADER-LEFTBOUNCE))
              false)
(check-expect (destroy-missile? MISSILE-FREE (list (make-invader (missile-x MISSILE-FREE) (missile-y MISSILE-FREE) 1)
                                                   INVADER-LEFTBOUNCE))
              true)

;; Stub:
;; (define (destroy-missile? invader lom) false)

;; Using template for ListOfInvaders
(define (destroy-missile? missile loi)
  (cond [(null? missile) true]
        [(empty? loi) false]
        [else (if (<= (dist-between (first loi) missile) (/ INVADER-HEIGHT 2))
                  (destroy-missile? null (rest loi))
                  (destroy-missile? missile (rest loi)))]))


;; =================
;; dist-between
;; =================

;; Invader Missile -> Number
;; Calculate the distance between and invader and a missile
(check-expect (dist-between INVADER-MID-LEFT (make-missile (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT)))
              0)
(check-expect (dist-between INVADER-MID-LEFT (make-missile (+ (invader-x INVADER-MID-LEFT) 30) (+ (invader-y INVADER-MID-LEFT) 40)))
              50)

;; Stub:
;; (define (dist-between invader lom) 0)

;; Using template for Invader:
(define (dist-between invader missile)
  (sqrt (+ (sqr (- (invader-x invader) (missile-x missile)))
           (sqr (- (invader-y invader) (missile-y missile))))))

;; =================
;; render-game
;; =================

;; Game -> Image
;; Render the game to an image!
(check-expect (render-game (make-game empty empty T0))
              (place-images/align (list TANK)
                                  (list (make-posn (tank-x T0) TANK-Y))
                                  "left" "top" BACKGROUND))
(check-expect (render-game (make-game (list INVADER-MID-LEFT) empty T0))
              (place-images/align (list INVADER TANK)
                                  (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                                        (make-posn (tank-x T0) TANK-Y))
                                  "left" "top" BACKGROUND))
(check-expect (render-game (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty T0))
              (place-images/align (list INVADER
                                        INVADER
                                        TANK)
                                 (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                                       (make-posn (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))
                                       (make-posn (tank-x T0) TANK-Y))
                                  "left" "top" BACKGROUND))
(check-expect (render-game (make-game empty (list MISSILE-FREE) T0))
              (place-images/align (list MISSILE
                                        TANK)
                                 (list (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                                       (make-posn (tank-x T0) TANK-Y))
                                  "left" "top" BACKGROUND))
(check-expect (render-game (make-game empty (list MISSILE-FREE MISSILE-HIT) T0))
              (place-images/align (list MISSILE
                                        MISSILE
                                        TANK)
                                 (list (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                                       (make-posn (missile-x MISSILE-HIT) (missile-y MISSILE-HIT))
                                       (make-posn (tank-x T0) TANK-Y))
                                  "left" "top" BACKGROUND))
(check-expect (render-game (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) (list MISSILE-FREE MISSILE-HIT) T0))
              (place-images/align (list INVADER
                                        INVADER
                                        MISSILE
                                        MISSILE
                                        TANK)
                                 (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                                       (make-posn (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))
                                       (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                                       (make-posn (missile-x MISSILE-HIT) (missile-y MISSILE-HIT))
                                       (make-posn (tank-x T0) TANK-Y))
                                  "left" "top" BACKGROUND))

;; Stub:
#;
(define (render-game game) (place-images/align (list TANK)
                                               (list (make-posn (tank-x T0) TANK-Y))
                                               "left" "top" BACKGROUND))

;; Using template for Game:
(define (render-game game)
  (place-images/align (generate-image-list game)
                      (generate-posn-list game)
                      "left" "top" BACKGROUND))

;; ====================
;; generate-image-list
;; ====================

;; Game -> ListOfImages
;; Produce a list of images from Game for rendering using place-imgages
(check-expect (generate-image-list (make-game empty empty T0))
              (list TANK))
(check-expect (generate-image-list (make-game (list INVADER-MID-LEFT) empty T0))
              (list INVADER TANK))
(check-expect (generate-image-list (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty T0))
              (list INVADER INVADER TANK))
(check-expect (generate-image-list (make-game empty (list MISSILE-FREE) T0))
              (list MISSILE TANK))
(check-expect (generate-image-list (make-game empty (list MISSILE-FREE MISSILE-HIT) T0))
              (list MISSILE MISSILE TANK))
(check-expect (generate-image-list (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE)
                                              (list MISSILE-FREE MISSILE-HIT)
                                              T0))
              (list INVADER INVADER MISSILE MISSILE TANK))

;; Stub:
;; (define (generate-image-list game) (list TANK))

;; Using template for Game:
(define (generate-image-list game)
  (append (generate-image-list-invader (game-invaders game) empty)
          (generate-image-list-missile (game-missiles game) empty)
          (list TANK)))


;; ===========================
;; generate-image-list-invader
;; ===========================

;; ListOfInvaders ListOfImages -> ListOfImages
;; Produce a list of images from ListOfInvaders
(check-expect (generate-image-list-invader empty empty) empty)
(check-expect (generate-image-list-invader (list INVADER-MID-LEFT) empty) (list INVADER))
(check-expect (generate-image-list-invader (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty) (list INVADER INVADER))

;; Stub:
;; (define (generate-image-list-invader loi loimg) empty)

;; Using template for ListOfInvaders:
(define (generate-image-list-invader loi loimg)
  (cond [(empty? loi) loimg]
        [else (cons INVADER (generate-image-list-invader (rest loi) loimg))]))


;; ===========================
;; generate-image-list-missile
;; ===========================

;; ListOfMissiles ListOfImages -> ListOfImages
;; Produce a list of images from ListOfMissiles
(check-expect (generate-image-list-missile empty empty) empty)
(check-expect (generate-image-list-missile (list MISSILE-FREE) empty) (list MISSILE))
(check-expect (generate-image-list-missile (list MISSILE-FREE MISSILE-HIT) empty) (list MISSILE MISSILE))

;; Stub:
;; (define (generate-image-list-missile lom loimg) empty)

;; Using template for ListOfMissiles
(define (generate-image-list-missile lom loimg)
  (cond [(empty? lom) loimg]
        [else (cons MISSILE (generate-image-list-missile (rest lom) loimg))]))


;; ====================
;; generate-posn-list
;; ====================

;; Game -> ListofPosns
;; Produce a list of images from Game for rendering using place-imgages
(check-expect (generate-posn-list (make-game empty empty T0)) (list (make-posn (tank-x T0) TANK-Y)))
(check-expect (generate-posn-list (make-game (list INVADER-MID-LEFT) empty T0))
              (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                    (make-posn (tank-x T0) TANK-Y)))
(check-expect (generate-posn-list (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty T0))
              (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                    (make-posn (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))
                    (make-posn (tank-x T0) TANK-Y)))
(check-expect (generate-posn-list (make-game empty (list MISSILE-FREE) T0))
              (list (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                    (make-posn (tank-x T0) TANK-Y)))
(check-expect (generate-posn-list (make-game empty (list MISSILE-FREE MISSILE-HIT) T0))
              (list (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                    (make-posn (missile-x MISSILE-HIT) (missile-y MISSILE-HIT))
                    (make-posn (tank-x T0) TANK-Y)))
(check-expect (generate-posn-list (make-game (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) (list MISSILE-FREE MISSILE-HIT) T0))
              (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                    (make-posn (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))
                    (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                    (make-posn (missile-x MISSILE-HIT) (missile-y MISSILE-HIT))
                    (make-posn (tank-x T0) TANK-Y)))

;; Stub:
;; (define (generate-posn-list game) (list (make-posn (tank-x T0) TANK-Y)))

;; Using template for Game:
(define (generate-posn-list game)
  (append (generate-posn-list-invader (game-invaders game) empty)
          (generate-posn-list-missile (game-missiles game) empty)
          (generate-posn-list-tank (game-tank game))))


;; ==========================
;; generate-posn-list-invader
;; ==========================

;; ListOfInvaders ListOfPosns -> ListOfPosns
;; Produce a list of images from Game for rendering using place-imgages
(check-expect (generate-posn-list-invader empty empty) empty)
(check-expect (generate-posn-list-invader (list INVADER-MID-LEFT) empty)
              (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))))
(check-expect (generate-posn-list-invader (list INVADER-MID-LEFT INVADER-LEFTBOUNCE) empty)
              (list (make-posn (invader-x INVADER-MID-LEFT) (invader-y INVADER-MID-LEFT))
                    (make-posn (invader-x INVADER-LEFTBOUNCE) (invader-y INVADER-LEFTBOUNCE))))

;; Stub:
;; (define (generate-posn-list-invader game loposn) empty)

;; Using template for ListOfInvaders
(define (generate-posn-list-invader loi loposn)
  (cond [(empty? loi) loposn]
        [else (cons (make-posn (invader-x (first loi)) (invader-y (first loi)))
                    (generate-posn-list-invader (rest loi) loposn))]))


;; ==========================
;; generate-posn-list-missile
;; ==========================

;; ListOfMissiles ListOfPosns -> ListOfPosns
;; Produce a list of images from Game for rendering using place-imgages
(check-expect (generate-posn-list-missile empty empty) empty)
(check-expect (generate-posn-list-missile (list MISSILE-FREE) empty)
              (list (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))))
(check-expect (generate-posn-list-missile (list MISSILE-FREE MISSILE-HIT) empty)
              (list (make-posn (missile-x MISSILE-FREE) (missile-y MISSILE-FREE))
                    (make-posn (missile-x MISSILE-HIT) (missile-y MISSILE-HIT))))

;; Stub:
;; (define (generate-posn-list-missile game loposn) empty)

;; Using template for ListOfMissiles
(define (generate-posn-list-missile lom loposn)
  (cond [(empty? lom) loposn]
        [else (cons (make-posn (missile-x (first lom)) (missile-y (first lom)))
                    (generate-posn-list-missile (rest lom) loposn))]))


;; =======================
;; generate-posn-list-tank
;; =======================

;; Tank -> ListOfPosn
;; Produce a list of images from Game for rendering using place-imgages
(check-expect (generate-posn-list-tank T0) (list (make-posn (tank-x T0) TANK-Y)))

;; Stub:
;; (define (generate-posn-list-tank tank) (list (make-posn (tank-x T0) TANK-Y)))

;; Using template for Tank
(define (generate-posn-list-tank tank)
  (list (make-posn (tank-x tank) TANK-Y)))


;; =================
;; update-lom-pos
;; =================

;; ListOfMissiles -> ListOfMissiles
;; Apply physics updates to the list of missiles
(check-expect (update-lom-pos empty) empty)
(check-expect (update-lom-pos (list MISSILE-FREE)) (list MISSILE-FREE-NEXT))
(check-expect (update-lom-pos (list MISSILE-FREE MISSILE-HIT)) (list MISSILE-FREE-NEXT MISSILE-HIT-NEXT))

;; Stub
;; (define (update-lom-pos lom) empty)

;; Using template for ListOfMissiles
(define (update-lom-pos lom)
  (cond [(empty? lom) empty]
        [else (cons (update-missile-pos (first lom))
                    (update-lom-pos (rest lom)))]))


;; ==================
;; update-missile-pos
;; ==================

;; Missile -> Missile
;; Apply physics update to a single missile
(check-expect (update-missile-pos MISSILE-FREE) MISSILE-FREE-NEXT)
(check-expect (update-missile-pos MISSILE-HIT) MISSILE-HIT-NEXT)

;; Stub:
;; (define (update-missile missile) MISSILE-FREE-NEXT)

;; Using template for Missile:
(define (update-missile-pos missile)
  (make-missile (missile-x missile) (- (missile-y missile) MISSILE-SPEED)))


;; =================
;; gameover-check
;; =================

;; Game -> Boolean
;; Check if the game is over
(check-expect (gameover-check (make-game empty empty T0)) false)
(check-expect (gameover-check (make-game (list (make-invader 50 (+ HEIGHT INVADER-HEIGHT) 1)) empty T0)) true)
(check-expect (gameover-check (make-game (list (make-invader 50 (+ HEIGHT INVADER-HEIGHT) 1) INVADER-MID-LEFT) empty T0)) true)

;; Stub:
;; (define (gameover-check game) false)

;; Using template from Game:
(define (gameover-check game)
  (invader-at-bottom? (game-invaders game)))


;; ==================
;; invader-at-bottom?
;; ==================

;; ListOfInvaders -> Boolean
;; Check if the game is over
(check-expect (invader-at-bottom? empty) false)
(check-expect (invader-at-bottom? (list (make-invader 50 (+ HEIGHT INVADER-HEIGHT) 1))) true)
(check-expect (invader-at-bottom? (list (make-invader 50 (+ HEIGHT INVADER-HEIGHT) 1) INVADER-MID-LEFT)) true)

;; Stub:
;; (define (invader-at-bottom? loi) false)

;; Using template for ListOfInvaders:
(define (invader-at-bottom? loi)
  (cond [(empty? loi) false]
        [else (if (>= (+ (invader-y (first loi)) INVADER-HEIGHT) HEIGHT)
                  true
                  (invader-at-bottom? (rest loi)))]))


;; ==========
;; handle-key
;; ==========

;; Game -> Game
;; Handle key presses:
;; * If left then move tank to the left
;; * If right then move tank to the right
;; * If spacebar then fire
(check-expect (handle-key (make-game empty empty T0) "x") (make-game empty empty T0))
(check-expect (handle-key (make-game empty empty (make-tank 100 0)) "left")
              (make-game empty empty (make-tank 100 -1)))
(check-expect (handle-key (make-game empty empty (make-tank 100 0)) "right")
              (make-game empty empty (make-tank 100 1)))

;; Stub:
;; (define (handle-key game key) game)

(define (handle-key game key)
  (cond [(key=? key " ") (make-game (game-invaders game)
                                    (fire-missile game)
                                    (game-tank game))]
        [(key=? key "left") (make-game (game-invaders game)
                                       (game-missiles game)
                                       (make-tank (tank-x (game-tank game)) -1))]
        [(key=? key "right") (make-game (game-invaders game)
                                       (game-missiles game)
                                       (make-tank (tank-x (game-tank game)) 1))]
        [else game]))


;; ============
;; fire-missile
;; ============

;; Game -> ListOfMissiles
;; Generate a new missile above the tank
(check-expect (fire-missile (make-game empty empty T0))
             (list (make-missile (tank-x T0) (- HEIGHT TANK-HEIGHT))))
(check-expect (fire-missile (make-game empty (list (make-missile (tank-x T0) (/ HEIGHT 2))) T0))
              (list (make-missile (tank-x T0) (- HEIGHT TANK-HEIGHT)) (make-missile (tank-x T0) (/ HEIGHT 2))))


;; Stub:
;; (define (fire-missile game) empty)

;; Using template for Tank:
(define (fire-missile game)
  (cons
   (make-missile (tank-x (game-tank game)) (- HEIGHT TANK-HEIGHT))
   (game-missiles game)))


;; ==============
;; handle-release
;; ==============

;; Game -> Game
;; If left or right key is released, reset tank direction
(check-expect (handle-release (make-game empty empty T0) "x") (make-game empty empty T0))
(check-expect (handle-release (make-game empty empty (make-tank 100 1)) "left")
              (make-game empty empty (make-tank 100 0)))
(check-expect (handle-release (make-game empty empty (make-tank 100 -1)) "right")
              (make-game empty empty (make-tank 100 0)))

;; Stub:
;; (define (handle-release game key) game)

(define (handle-release game key)
  (cond [(or (key=? key "left") (key=? key "right"))
         (make-game (game-invaders game)
                    (game-missiles game)
                    (make-tank (tank-x (game-tank game)) 0))]
        [else game]))
```

## Resources

*  The Career Development Series > Technical Interviews > What to Expect in A Technical Interview
