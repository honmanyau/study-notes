# How to Code: Simple Data

## Table of Contents

* [How to Code: Simple Data](#how-to-code-simple-data)
  * [Notes](#notes)
  * [Exercises](#exercises)
  * [Questions](#questions)

## Notes

### 0: Introduction

* It's hard to write a program because it's usually given as a poorly-formed problem
* Figure out exactly what it is that we want the program to do first
* Then learn to break down the problem into well-choosen pieces first that can be well-tested and maintained and modified
* BSL (Beginning Student Language)

### 1a: Beginning Student Language

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

BSL P16—Foo Evaluation

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

### 1a: Beginning Student Language

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


### Recommended Problems

#### HtDF P2 - Less Than 5

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

#### HtDF P3 - Boxify

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

#### HtDF P6 - Double Error

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

```
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

## Questions
