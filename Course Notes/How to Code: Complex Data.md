# How to Code: Simple Data

## Table of Contents

* [How to Code: Simple Data](#how-to-code-simple-data)
  * [Notes](#notes)
  * [Resources](#resources)

## Notes

### Module 7: Mutual Reference

#### Templating Mutual Recursion

* When translating from type comments to template, where there is a SR, there is an NR, where there is an MR, there is an NMR

#### Functions on Mutually Recursive Data - Part 1

* Whoa, this is amazing



Problem attempts:

> Design a function that consumes Element and produces the sum of all the file data in
the tree.

Problem covered in the video, attempting for practice.

```BSL
;; Element -> Integer
;; ListOfElements -> Integer
(check-expect (sum-of-eles--element (make-elt "T0" 1 empty)) 1)
(check-expect (sum-of-eles--element F1) 1)
(check-expect (sum-of-eles--element D5) 3)
(check-expect (sum-of-eles--element D4) 3)
(check-expect (sum-of-eles--element D6) 6)


;; Stub:
;; (define (sum-of-eles--element loe) 0)
;; (define (sum-of-eles--loe loe) 0)

;; Using template provided for Element + ListOfElements:
(define (sum-of-eles--element e)
  (if (zero? (elt-data e))
      (sum-of-eles--loe (elt-subs e))
      (elt-data e)))

(define (sum-of-eles--loe loe)
  (cond [(empty? loe) 0]
        [else
         (+ (sum-of-eles--element (first loe))
            (sum-of-eles--loe (rest loe)))]))

```

> Design a function that consumes Element and produces a list of the names of all the elements in
the tree.

```BSL
;; Element -> ListOfNames
;; ListOfElements -> ListOfNames
;; Produces a list of all the names in the tree described by the given node
(check-expect (list-names--loe empty) (list empty))
(check-expect (list-names--element empty) empty)
(check-expect (list-names--element F3) (list "F3"))
(check-expect (list-names--element D5) (list "D5" "F3"))
(check-expect (list-names--element D4) (list "D4" "F1" "F2"))
(check-expect (list-names--element D6) (list "D6" "D4" "F1" "F2" "D5" "F3"))

;; Stub:
;; (define (list-names--element e) empty)
;; (define (list-names--loe loe) empty)

;; Using template provided for Element + ListOfElements:
(define (list-names--element e)
  (cons (elt-name e)    ;String
        (list-names--loe (elt-subs e))))

(define (list-names--loe loe)
  (cond [(empty? loe) empty]
        [else
         (append (list-names--element (first loe))
                 (list-names--loe (rest loe)))]))
```

#### Multiple Choice Quiz

Suppose we have the following structure:

```BSL
(define-struct nya (a b))
```

There is no MR in the case below even though nya is in its definition `nya`:

```BSL
;; Nyu is one of:
;; * false
;; * (make-nya Nyu)
```

### 8a: Two One-of Types

#### Cross Product Table

* When two of a one-of type is used, the cross product of the subtypes in their type comments is helpful in determining the minimum amount of examples that need to be written


#### Cross Product Code

* Cross product table can also help with template-writing; however, instead of writing a `cond` case for each of the product, examine the possible examples for similarities (hopefully written correctly and exhaustively) and simplify the cases first

**Comment**

It seems like the examples are starting to get complex enough such that it's starting to deviate from the simple templating approach (without reasoning about what should and shouldn't got into the template anymore). Hmm...

#### Recommended Problems

**2 One-Of P2 - Merge**

```BSL
;; ListOfNumbers is one of:
;; * empty
;; * (cons Number ListOfNumbers)
;; interp. a list of numbers that are sorted in ascending order

(define LON0 empty);
(define LON1 (list 1));
(define LON2 (list 2));
(define LON37 (list 3 7));
(define LON49 (list 4 9));

;; Template rules:
;; * One of: 2 cases
;; * Atomic-distinct: empty
;; * Compound (cons Number ListOfNumbers)
;; * Self-reference: (rest lon) is ListOfNumbers
(define (fn-for-lon lon)
  (cond [(empty? lon) (...)]
        [else (... (first lon)
                   (fn-for-lon (rest lon)))]))

;; =================
;; Function
;; =================

;; The function merge takes two ListOfNumbers and produces one ListOfNumbers, where
;; ListOfNumbers is sorted in ascending order.

;; Signature:
;; ListOfNumbers ListOfNumbers -> ListOfNumbers

;; Cross Product Table:
;; |       | empty |       lona       |
;; |-------|-------|------------------|
;; | empty | empty |       lonb       |
;; | lonb  | lona  | merge lona lonb  |
;;
;; The case of empty empty doesn't actually need to be handled
;; because empty [(empty? lona) lonb] actually already takes
;; care of it and vice versa.

;; Examples:
(check-expect (merge empty empty) empty)
(check-expect (merge LON1 empty) LON1)
(check-expect (merge LON37 empty) LON37)
(check-expect (merge empty LON1) LON1)
(check-expect (merge empty LON37) LON37)
(check-expect (merge LON1 LON2) (append LON1 LON2))
(check-expect (merge LON2 LON1) (append LON1 LON2))
(check-expect (merge LON1 LON49) (cons 1 (cons 4 (cons 9 empty))))
(check-expect (merge LON49 LON1) (cons 1 (cons 4 (cons 9 empty))))
(check-expect (merge LON37 LON49) (cons 3 (cons 4 (cons 7 (cons 9 empty)))))
(check-expect (merge LON49 LON37) (cons 3 (cons 4 (cons 7 (cons 9 empty)))))

;; Stub:
;; (define (merge lona lonb) empty)

;; Template
#;
(define (merge lona lonb)
  (cond [(empty? lona) lonb]
        [(empty? lonb) lona]
        [else (... (first lona)
                   (first lonb)
                   (merge (rest lona) (rest lonb)))]))

(define (merge lona lonb)
  (cond [(empty? lona) lonb]
        [(empty? lonb) lona]
        [else (if (< (first lona) (first lonb))
                  (append (list (first lona) (first lonb)) (merge (rest lona) (rest lonb)))
                  (append (list (first lonb) (first lona)) (merge (rest lona) (rest lonb))))]))
```

**2 One-Of P4 - Pattern Match**

```BSL
;; Function pattern-match? checks whether or a given Pattern and ListOf1String
;; are complements according tothe rules described above

;; Signature:
;; Pattern ListOf1String -> Boolean

;; Cross Product Table:
;; |                              | empty |  (cons "A" Pattern)  |  (cons "N" Pattern)  |
;; |------------------------------|-------|----------------------|----------------------|
;; |             empty            | true  |         false        |         false        |
;; | (cons 1String ListOf1String) | false | pattern-match? p los | pattern-match? p los |

(check-expect (pattern-match? empty empty) true)
(check-expect (pattern-match? (list "N") empty) false)
(check-expect (pattern-match? (list "N") empty) false)
(check-expect (pattern-match? empty (list "1")) false)
(check-expect (pattern-match? empty (list "1" "2")) false)
(check-expect (pattern-match? (list "A") empty) false)
(check-expect (pattern-match? empty (list "X")) false)
(check-expect (pattern-match? (list "N") (list "1")) true)
(check-expect (pattern-match? (list "A") (list "1")) false)
(check-expect (pattern-match? (list "A") (list "X")) true)
(check-expect (pattern-match? (list "N") (list "X")) false)
(check-expect (pattern-match? (list "N" "A") (list "1" "X")) true)
(check-expect (pattern-match? (list "A" "N") (list "X" "1")) true)
(check-expect (pattern-match? (list "N" "A") (list "X" "1")) false)
(check-expect (pattern-match? (list "A" "N") (list "1" "X")) false)
(check-expect (pattern-match? (list "A" "N" "A" "N" "A" "N")
                              (list "V" "6" "T" "1" "Z" "4")) true)

;; Stub:
;; (define (pattern-match? p los) true)

;; Template:
#;
(define (pattern-match? p los)
  (cond [(and (empty? p) (empty? los)) true]
        [(empty? p) false]
        [(empty? los) false]
        [(... (first p)
              (first los)
              (pattern-match? (rest p) (rest los)))]))

(define (pattern-match? p los)
  (cond [(and (empty? p) (empty? los)) true]
        [(empty? p) false]
        [(empty? los) false]
        [(string=? (first p) "A")
         (if (alphabetic? (first los))
             (pattern-match? (rest p) (rest los))
             false)]
        [(string=? (first p) "N")
         (if (numeric? (first los))
             (pattern-match? (rest p) (rest los))
             false)]))
```

### Module 8b: Local

#### Avoid Recomputation

**Local P3 - Evaluate Foo**

```ISL
;; Start
#;
(define (foo n)
  (local [(define x (* 3 n))]
    (if (even? x)
        n
        (+ n (foo (sub1 n))))))

#;
(foo 3)


;; Step 1
#;
(local [(define x (* 3 3))]
  (if (even? x)
      3
      (+ 3 (foo (sub1 3)))))


;; Step 2
#;
(local [(define x 9)]
  (if (even? x)
      3
      (+ 3 (foo (sub1 3)))))


;; Step 3
#;
(define x_0 9)

#;
(if (even? x_0)
    3
    (+ 3 (foo (sub1 3))))


;; Step 4
#;
(define x_0 9)

#;
(if false
    3
    (+ 3 (foo (sub1 3))))


;; Step 5
#;
(define x_0 9)

#;
(+ 3 (foo (sub1 3)))


;; Step 6
#;
(define x_0 9)

#;
(+ 3 (foo (sub1 3)))


;; Step 7
#;
(define x_0 9)

#;
(+ 3 (foo 2))


;; Step 8
#;
(define x_0 9)

#;
(+ 3 (local [(define x (* 3 2))]
       (if (even? x)
           2
           (+ 2 (foo (sub1 2))))))


;; Step 9
#;
(define x_0 9)

#;
(+ 3 (local [(define x (* 3 2))]
       (if (even? x)
           2
           (+ 2 (foo (sub1 2))))))


;; Step 10
#;
(define x_0 9)

#;
(+ 3 (local [(define x 6)]
       (if (even? x)
           2
           (+ 2 (foo (sub1 2))))))


;; Step 11
#;
(define x_0 9)
(define x_1 6)

#;
(+ 3 (if (even? x_1)
         2
         (+ 2 (foo (sub1 2)))))


;; Step 12
#;
(define x_0 9)
(define x_1 6)

#;
(+ 3 (if true
         2
         (+ 2 (foo (sub1 2)))))


;; Step 13
#;
(define x_0 9)
(define x_1 6)

#;
(+ 3 2)


;; Step 14
#;
(define x_0 9)
(define x_1 6)

#;
(+ 5)
```

**Local P5 - Improved Championship Bracket**

```ISL
(define (ouster t br)
  (cond [(false? br) false]
        [else
         (if (string=? t (bracket-team-lost br))
             (bracket-team-won br)
             (local [(define bbw (ouster t (bracket-br-won br)))] (if (not (false? bbw))
                 bbw
                 (ouster t (bracket-br-lost br)))))]))

;; Reason for replacement—the expression (ouster t (bracket-br-won br))
;; in the original function and one of them was involved in recursion and
;; was used twice; replacing it removes the need for it to be recomputed
;; in subsequent recursive calls.
```

#### Design Quiz

**Problem 1**

```ISL
;; ========
;; Function
;; ========

;; Determine whether or not all the players in two different tennis teams,
;; as given by two rosters of players, one for each team. If so, return true;
;; otherwise, return false.

;; Signature:
;; Roster Roster -> Boolean

;; Cross Product Table:
;; |                      | empty |  (cons Player Roster)  |
;; |----------------------|-------|------------------------|
;; |         empty        | true  |         false          |
;; | (cons Player Roster) | false |    all-play? r1 r2     |

(check-expect (all-play? empty empty) true)
(check-expect (all-play? (list "Nadeshiko") empty) false)
(check-expect (all-play? empty (list "Shirayuri")) false)
(check-expect (all-play? (list "Nadeshiko") (list "Shirayuri")) true)
(check-expect (all-play? (list "Nadeshiko" "Renge") (list "Shirayuri")) false)
(check-expect (all-play? (list "Nadeshiko" "Renge") (list "Shirayuri" "Ayame")) true)
(check-expect (all-play? (list "Nadeshiko" "Renge" "Tsubaki") (list "Shirayuri" "Ayame")) false)

;; Stub:
;; (define (all-play? r1 r2) false)

;; Using template from Roster:
(define (all-play? r1 r2)
  (cond [(and (empty? r1) (empty? r2)) true]
        [(or (empty? r1) (empty? r2)) false]
        [else (all-play? (rest r1) (rest r2))]))
```

```ISL
;; ========
;; Function
;; ========

;; Given two Rosters, produce a list of all parings (ListOfMatch) for each
;; pair of players.


;; Signature:
;; Roster Roster -> ListOfMatch

;; Cross Product Table:
;; |                      |      empty       |  (cons Player Roster)  |
;; |----------------------|------------------|------------------------|
;; |         empty        |      empty       |    --not possible--    |
;; | (cons Player Roster) | --not possible-- |    match-all r1 r2     |

(check-expect (match-all empty empty) empty)
(check-expect (match-all (list "Nadeshiko") (list "Shirayuri"))
              (list (make-match "Nadeshiko" "Shirayuri")))
(check-expect (match-all (list "Nadeshiko" "Renge") (list "Shirayuri" "Ayame"))
              (list (make-match "Nadeshiko" "Shirayuri")
                    (make-match "Renge" "Ayame")))

;; Stub:
;; (define (match-all r1 r2) empty)

;; Using template for ListOfMatch:
(define (match-all r1 r2)
  (cond [(and (empty? r1) (empty? r2)) empty]
        [else (cons (make-match (first r1) (first r2))
                    (match-all (rest r1) (rest r2)))]))
```

## Resources
