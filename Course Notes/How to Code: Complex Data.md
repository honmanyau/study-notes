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

## Resources
