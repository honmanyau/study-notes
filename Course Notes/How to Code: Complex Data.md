# How to Code: Simple Data

## Table of Contents

* [How to Code: Simple Data](#how-to-code-simple-data)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)
  * [Resources](#resources)

## Disclaimer

Everything in this file is derived from going through [How to Code: Complex Data](https://www.edx.org/course/how-code-complex-data-ubcx-htc2x) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

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

**Remarks after finishing module 8**

The comment made at the beginning of the module about how things seem to start deviation from template is certainly true but because of how tightly everything is coupled and the use of cross product table, nothing much has actually changed and everything just falls in place again.

### Module 9: Abstraction

#### From Examples 1

* Prof mentioned that almost no other language have higher-order function syntax as simple as Racket; but JavaScript does!

#### Recommended Problems

**Abstraction P1 - Wide Only**

```ISL
;; =========
;; Functions
;; =========

;; Signature:
;; Image -> Boolean

;; Given an image, check whether or not its width is greater than its height.
;; If so, return true, else return false.
(check-expect (wider? empty-image) false);
(check-expect (wider? (rectangle 20 10 "solid" "white")) true);
(check-expect (wider? (rectangle 10 20 "solid" "white")) false);

;; Stub:
;; (define (wider? img) false)

;; Template rules:
;; * Atomic-non-distinct: Image
#;
(define (wider? img) (... img))

(define (wider? img)
  (if (> (image-width img) (image-height img))
      true
      false))

;; Signature
;; (listof Image) -> (listof Image)

;; With a list of images as input, return those that are only wider than
;; they are tall.
(check-expect (wide-only empty) empty)
(check-expect (wide-only (list (rectangle 10 20 "solid" "white"))) empty)
(check-expect (wide-only (list (rectangle 20 10 "solid" "white"))) (list (rectangle 20 10 "solid" "white")))
(check-expect (wide-only (list (rectangle 20 10 "solid" "white") (rectangle 10 20 "solid" "white")))
              (list (rectangle 20 10 "solid" "white")))
(check-expect (wide-only (list (rectangle 20 10 "solid" "white") (rectangle 30 10 "solid" "white")))
              (list (rectangle 20 10 "solid" "white") (rectangle 30 10 "solid" "white")))
(check-expect (wide-only (list (rectangle 10 20 "solid" "white") (rectangle 10 30 "solid" "white")))
              empty)

;; Stub:
;; (define (wide-only loi) empty-image)

;; Using filter
(define (wide-only loi) (filter wider? loi))
```

**Abstraction P8 - Fold Directory**

> Design an abstract fold function for Dir called fold-dir.

Problem 1:

```ISL
;; Template for Dir:
#;
(define (fn-for-dir dir)
  (... (dir-name dir)
       (fn-for-lod (dir-sub-dirs dir))
       (fn-for-loi (dir-images dir))))

;; Template for ListOfDir:
#;
(define (fn-for-lod lod)
  (cond [(empty? lod) (...)]
        [else (... (fn-for-dir (first lod))
                   (fn-for-lod (rest lod)))]))

;; Template for ListOfImage:
#;
(define (fn-for-loi loi)
  (cond [(empty? loi) (...)]
        [else (... (first loi)
                   (fn-for-loi (rest loi)))]))

;; Not entirely clear what the signature should be yet, but since
;; it's based on Dir, it must be able to produce a Dir
(check-expect (fold-dir make-dir cons cons empty empty EMPTY-DIR) EMPTY-DIR)
;; It should also be able to iterate through all images found and
;; return the sum of their height
(check-expect (local [(define (p1 dirname lod loi) (+ lod loi))
                      (define (p2 dir lod) (+ dir lod))
                      (define (p3 img cb) (+ (image-height img) cb))]
                (fold-dir p1 p2 p3 0 0 DIR3))
              (+ (* (image-height IMG1) 2) (image-height IMG2) (image-height IMG3)))

;; Use template for Dir and expand with ListOfDir and ListOfImages,
;; Replace lod with (dir-sub-dirs dir)

(define (fold-dir p1 p2 p3 x y dir)
  (local [;; Template for Dir
          (define (fn-for-dir dir)
            (p1 (dir-name dir)
                (fn-for-lod (dir-sub-dirs dir))
                (fn-for-loi (dir-images dir))))
          ;; Template for ListOfDir
          (define (fn-for-lod lod)
            (cond [(empty? lod) x]
                  [else (p2 (fn-for-dir (first lod))
                            (fn-for-lod (rest lod)))]))
          ;; Template for ListOfImage
          (define (fn-for-loi loi)
            (cond [(empty? loi) y]
                  [else (p3 (first loi)
                            (fn-for-loi (rest loi)))]))]
    (fn-for-dir dir)))

;; After the fact signature:
;; According to the parameters, where p1 p2 and p3 are functions
;; and x and y are return values inside the function, we have:

;; 1. () () () X Y Dir -> ???


;; p3 has the signature Image Y -> Y because fn-for-loi has to return y

;; 2. () () (Image Y -> Y) X Y Dir -> ???


;; p2 has the signature of ??? X -> X for similar reasons, but it's not
;; clear what ??? should be yet since fn-for-dir is operating on dir
;; and the first argument to p2 is (fn-for-dir dir)

;; 3. () (??? X -> X) (Image Y -> Y) X Y Dir -> ???


;; p1 obviously returns ??? (   -> ???). With reference to the type
;; coment for Dir, its signature is therefore String Y X -> ???.
;; As such, as have:

;; 4. (String Y X -> ???) (??? X -> X) (Image Y -> Y) X Y Dir -> ???


;; At this point it doesn't matter what ??? is, it's just some unknown
;; thing, so let's call it Z!

;; 5. (String Y X -> Z) (Z X -> X) (Image Y -> Y) X Y Dir -> Z
```

Problem 2:

> Design a function that consumes a Dir and produces the number of images in the directory and its sub-directories. Use the fold-dir abstract function.

```ISL
;; Signature:
;; DIR -> Natural

;; Examples:
(check-expect (count-images EMPTY-DIR) 0)
(check-expect (count-images DIR1) 1)
(check-expect (count-images DIR2) 2)
(check-expect (count-images DIR3) 3)
(check-expect (count-images DIR4) 4)

;; Stub:
;; (define (count-images dir) 0)

;; Refer to problem 1:
(define (count-images dir)
  (local [(define (p1 dirname cblod cbloi) (+ cblod cbloi))
          (define (p2 cbdir cblod) (+ cbdir cblod))
          (define (p3 img cbloi) (+ 1 cbloi))]
    (fold-dir p1 p2 p3 0 0 dir)))

```

Problem 3:

> Design a function that consumes a Dir and a String. The function looks in dir and all its sub-directories for a directory with the given name. If it finds such a directory it should produce true, if not it should produce false. Use the fold-dir abstract function.

```ISL
;; Signature:
;; Dir String -> Boolean

;; Examples:
(check-expect (dir-exist? EMPTY-DIR "") false)
(check-expect (dir-exist? DIR1 "DIR1") true)
(check-expect (dir-exist? DIR3 "DIR3") true)
(check-expect (dir-exist? DIR3 "DIR2") true)
(check-expect (dir-exist? DIR3 "DIR1") false)
(check-expect (dir-exist? DIR4 "DIR4") true)
(check-expect (dir-exist? DIR4 "DIR3") true)
(check-expect (dir-exist? DIR4 "DIR2") true)
(check-expect (dir-exist? DIR4 "DIR1") false)
(check-expect (dir-exist? DIR6 "DIR6") true)
(check-expect (dir-exist? DIR6 "DIR5") true)
(check-expect (dir-exist? DIR6 "DIR4") true)
(check-expect (dir-exist? DIR6 "DIR3") true)
(check-expect (dir-exist? DIR6 "DIR2") true)
(check-expect (dir-exist? DIR6 "DIR1") false)

;; Stub:
;; (define (dir-exist? dir name) false)

;; Refer to problem 1:
(define (dir-exist? dir name)
  (local [(define (p1 dirname cblod cbloi)
            (or (string=? dirname name)
                cblod))
          (define (p2 cbdir cblod) (or cbdir cblod))
          (define (p3 img cbloi) cbloi)]
    (fold-dir p1 p2 p3 false empty dir)))

#;
(define (fold-dir p1 p2 p3 x y dir)
  (local [;; Template for Dir
          (define (fn-for-dir dir)
            (p1 (dir-name dir)
                (fn-for-lod (dir-sub-dirs dir))
                (fn-for-loi (dir-images dir))))
          ;; Template for ListOfDir
          (define (fn-for-lod lod)
            (cond [(empty? lod) x]
                  [else (p2 (fn-for-dir (first lod))
                            (fn-for-lod (rest lod)))]))
          ;; Template for ListOfImage
          (define (fn-for-loi loi)
            (cond [(empty? loi) y]
                  [else (p3 (first loi)
                            (fn-for-loi (rest loi)))]))]
    (fn-for-dir dir)))
```

Problem 4:

```ISL
;; Probably not. At least with reference to the answer given for problem 3,
;; the use of or implies that even if the folder name is already found, the
;; algorithm will still recursively walk through the "rest" of the folder
;; structure.
```

### Quiz

**Problem 1**

```ISL
;; ========
;; Solution
;; ========

(define (arrange-all fn img loi)
  (cond [(empty? loi) img]
        [else
         (fn (first loi)
             (arrange-all fn img (rest loi)))]))
```

**Problem 2**

```ISL
;; ==========
;; Function 1
;; ==========

(define (lengths lst) (map string-length lst))


;; ==========
;; Function 2
;; ==========

(define (odd-only lon) (filter odd? lon))


;; ==========
;; Function 3
;; ==========

(define (all-odd? lon) (andmap odd? lon))


;; ==========
;; Function 4
;; ==========

(define (minus-n lon n) (foldr + n lon))
```

**Problem 3**

```ISL
(define (fold-region p1 p2 a b c d e f r)
  (local [(define (fold-region r)
            (p1 (region-name r)
                (fn-for-type (region-type r))
                (fn-for-lor (region-subregions r))))

          (define (fn-for-type t)
            (cond [(string=? t "Continent") a]
                  [(string=? t "Country") b]
                  [(string=? t "Province") c]
                  [(string=? t "State") d]
                  [(string=? t "City") e]))

          (define (fn-for-lor lor)
            (cond [(empty? lor) f]
                  [else
                   (p2 (fold-region (first lor))
                       (fn-for-lor (rest lor)))]))]
    (fold-region r)))

(define (all-regions region)
  (local [(define (p1 name typecb lorcb) (cons name lorcb))]
    (fold-region p1 append "" "" "" "" "" empty region)))
```

### Module 10a: Generative Recursion

#### Fractals, pt. 1

Sierpinski carpet problem attempt:

```ISL
;; =========
;; Constants
;; =========

(define CUTOFF 3)
(define BASESQUARE (square CUTOFF "outline" "red"))

;; =========
;; Function
;; =========

;; Number -> Image
;; Produce a Sierpinski carpet of size s
(check-expect (sarpet CUTOFF) BASESQUARE)
(check-expect (sarpet (* CUTOFF 3))
              (overlay (square (* CUTOFF 3) "outline" "red")
                       (above (beside BASESQUARE
                                      BASESQUARE
                                      BASESQUARE)
                              (beside BASESQUARE
                                      (square CUTOFF "solid" "white")
                                      BASESQUARE)
                              (beside BASESQUARE
                                      BASESQUARE
                                      BASESQUARE))))

;; Stub:
;; (define (sarpet size) BASESQUARE)

;; Using template for generative recursion:
(define (sarpet size)
  (if (> size CUTOFF)
      (local [(define inner-size (/ size 3))
              (define inner-sq (sarpet inner-size))]
        (overlay (square size "outline" "red")
                 (above (beside inner-sq
                                inner-sq
                                inner-sq)
                        (beside inner-sq
                                (square inner-size "solid" "white")
                                inner-sq)
                        (beside inner-sq
                                inner-sq
                                inner-sq))))
      (square size "outline" "red")))
```

#### Termination Arguments, pt 1

**Three-part termination argument**

* Identify base case
* Identify reduction step
* Reason about how repeated application of the reductio step will eventually lead to the base case

#### Recommended Problems

**Generative Recursion P1 - Circle Fractal**

```ISL
;; =================
;; Constants:

(define STEP (/ 2 5))
(define TRIVIAL-SIZE 5)
(define BASECIRCLE (circle TRIVIAL-SIZE "solid" "blue"))

;; Number -> Image
;; Produce a fractal image where each circle is surrounded by circles that are
;; two-fifths smaller of the given size
(check-expect (fractal-branch TRIVIAL-SIZE "blue") BASECIRCLE)
(check-expect (fractal-branch (/ TRIVIAL-SIZE STEP) "blue")
              (local [(define upstepped (/ TRIVIAL-SIZE STEP))]
                (above (circle TRIVIAL-SIZE "solid" "black")
                       (beside (rotate 90 (circle TRIVIAL-SIZE "solid" "red"))
                               (circle upstepped "solid" "blue")
                               (rotate -90 (circle TRIVIAL-SIZE "solid" "green"))))))

;; Stub:
;; (define (circle-fractal size) BASECIRCLE)

;; Using template for generative recursion:

(define (fractal-branch size c)
  (if (<= size TRIVIAL-SIZE)
      (circle size "solid" c)
      (local [(define ssize (* size STEP))
              (define (scircle c) (fractal-branch ssize c))]
        (above (scircle "black")
               (beside (rotate 90 (scircle "red")) (circle size "solid" "blue") (rotate -90 (scircle "green")))))))


;; Number -> Image
;; Produce a fractal image where each circle is surrounded by circles that are
;; two-fifths smaller of the given size
(check-expect (fractal TRIVIAL-SIZE) BASECIRCLE)
(check-expect (fractal (/ TRIVIAL-SIZE STEP))
              (local [(define upstepped (/ TRIVIAL-SIZE STEP))]
                (beside (rotate 90 BASECIRCLE)
                        (rotate 90 (beside (rotate 90 BASECIRCLE)
                                           (circle upstepped "solid" "blue")
                                           (rotate -90 BASECIRCLE)))
                        (rotate -90 BASECIRCLE))))


;; Stub:
;; (define (fractal size) BASECIRCLE)

;; Putting everything together into one image
(define (fractal size)
  (if (<= size TRIVIAL-SIZE)
      (circle size "solid" "blue")
      (local [(define ssize (* size STEP))]
        (beside (rotate 90 (fractal-branch ssize "blue"))
                (rotate 90 (beside (rotate 90 (fractal-branch ssize "blue"))
                                   (circle size "solid" "blue")
                                   (rotate -90 (fractal-branch ssize "blue"))))
                (rotate -90 (fractal-branch ssize "blue"))))))
```

### Module 10b: Search

#### Quiz

```ISL
(require 2htdp/image)
(require racket/list)

;  PROBLEM 1:
;  
;  In the lecture videos we designed a function to make a Sierpinski triangle fractal.
;  
;  Here is another geometric fractal that is made of circles rather than triangles:
;  
;  .
;  
;  Design a function to create this circle fractal of size n and colour c.
;  


(define CUT-OFF 5)

;; Natural String -> Image
;; produce a circle fractal of size n and colour c
(check-expect (circle-fractal CUT-OFF "black")
              (circle CUT-OFF "outline" "black"))
(check-expect (circle-fractal CUT-OFF "red")
              (circle CUT-OFF "outline" "red"))
(check-expect (circle-fractal (* CUT-OFF 2) "black")
              (overlay (beside (circle CUT-OFF "outline" "black")
                               (circle CUT-OFF "outline" "black"))
                       (circle (* CUT-OFF 2) "outline" "black")))

;; Stub:
;; (define (circle-fractal n c) empty-image)

;; Using template for generative recursion:
(define (circle-fractal n c)
  (cond [(<= n CUT-OFF) (circle n "outline" c)]
        [else (local [(define inner-circle (circle-fractal (/ n 2) c))]
                (overlay (beside inner-circle inner-circle)
                         (circle n "outline" c)))]))



;  PROBLEM 2:
;  
;  Below you will find some data definitions for a tic-tac-toe solver.
;  
;  In this problem we want you to design a function that produces all
;  possible filled boards that are reachable from the current board.
;  
;  In actual tic-tac-toe, O and X alternate playing. For this problem
;  you can disregard that. You can also assume that the players keep
;  placing Xs and Os after someone has won. This means that boards that
;  are completely filled with X, for example, are valid.
;  
;  Note: As we are looking for all possible boards, rather than a winning
;  board, your function will look slightly different than the solve function
;  you saw for Sudoku in the videos, or the one for tic-tac-toe in the
;  lecture questions.
;  


;; Value is one of:
;; - false
;; - "X"
;; - "O"
;; interp. a square is either empty (represented by false) or has and "X" or an "O"

(define (fn-for-value v)
  (cond [(false? v) (...)]
        [(string=? v "X") (...)]
        [(string=? v "O") (...)]))

;; Board is (listof Value)
;; a board is a list of 9 Values
(define B0 (list false false false
                 false false false
                 false false false))

(define B1 (list false "X"   "O"   ; a partly finished board
                 "O"   "X"   "O"
                 false false "X"))

(define B2 (list "X"  "X"  "O"     ; a board where X will win
                 "O"  "X"  "O"
                 "X" false "X"))

(define B3 (list "X" "O" "X"       ; a board where Y will win
                 "O" "O" false
                 "X" "X" false))

(define (fn-for-board b)
  (cond [(empty? b) (...)]
        [else
         (... (fn-for-value (first b))
              (fn-for-board (rest b)))]))


;; ======================
;; Constants for testing
;; ======================

(define TB0 (list "X" "O" "X"
                  "O" "O" "O"
                  "X" "X" "O"))

(define TB1 (list false "O" "X"
                  "O"   "O" "O"
                  "X"   "X" "O"))

(define TB1a (list "X" "O" "X"
                   "O" "O" "O"
                   "X" "X" "O"))

(define TB1b (list "O" "O" "X"
                   "O" "O" "O"
                   "X" "X" "O"))

(define TB2 (list false false "X"
                  "O"   "O"   "O"
                  "X"   "X"   "O"))

(define TB2a (list "X" "X" "X"
                   "O" "O" "O"
                   "X" "X" "O"))

(define TB2b (list "X" "O" "X"
                   "O" "O" "O"
                   "X" "X" "O"))

(define TB2c (list "O" "X" "X"
                   "O" "O" "O"
                   "X" "X" "O"))

(define TB2d (list "O" "O" "X"
                   "O" "O" "O"
                   "X" "X" "O"))

(define TB3 (list "X" "X" "X"
                  "O" "O" "O"
                  "X" "X" "O"))

(define TB4 (list "X" "O" "X"
                  "O" "X" "O"
                  "X" "X" "O"))

;; =========
;; Functions
;; =========

;; Board -> (listof Board)
;; For a given board, produce a list of all possible FILLED
;; boards, including ones that are normally not considered
;; valid (all grids filled with the same sign, for example).
(check-expect (gen-boards TB0) (list TB0))
(check-expect (gen-boards TB1) (list TB1a TB1b))
(check-expect (gen-boards TB2) (list TB2a TB2b TB2c TB2d))

;; Stub:
;; (define (gen-boards board) empty)

;; Using combined template for arbitrary-arity tree and generative recursion
(define (gen-boards board)
  (local [(define (solve--bd board)
            (if (board-full? board) ;; When the board is full, no more board should be generated, hence empty
                (list board)
                (solve--lobd (next-boards board))))
          (define (solve--lobd lobd)
            (cond [(empty? lobd) empty]
                  [else
                   (append (solve--bd (first lobd))
                           (solve--lobd (rest lobd)))]))]
    (solve--bd board)))


;; Board -> Boolean
;; Determine if the given board is full
(check-expect (board-full? TB0) true)
(check-expect (board-full? TB1) false)
(check-expect (board-full? TB1a) true)
(check-expect (board-full? TB2) false)
(check-expect (board-full? TB2a) true)

;; (define (board-full? board) false)

;; Using andmap primitive:
(define (board-full? board) (andmap string? board))


;; Board -> (listof Boards)
;; Generate the next possible boards for the given board
(check-expect (next-boards TB1) (list TB1a TB1b))
(check-expect (next-boards TB2) (list (cons "X" (rest TB2))
                                      (cons "O" (rest TB2))))

;; Stub:
;; (define (next-boards board) empty)

;; Function composition:
(define (next-boards board)
  (fill-board (find-empty-pos board) board))
#;
(define (next-boards board)
  (cond [(empty? board) (error "Something went horribly wrong!")]
        [else
         (if (false? (first board))
             (list (cons "X" (rest board)) (cons "O" (rest board)))
             (cons (first board) (next-boards (rest board))))]))


;; Board -> Index
;; Return the index of the board position to be filled
(check-expect (find-empty-pos TB1) 0)
(check-expect (find-empty-pos (cons "X" (rest TB2))) 1)

;; Stub:
;; (define (find-empty-pos board) 0)

;; Using template for board
(define (find-empty-pos board)
  (cond [(empty? board) (error "Something went horribly wrong!")]
        [else
         (if (false? (first board))
             0
             (+ 1 (find-empty-pos (rest board))))]))


;; Index Board -> (listof Board)
;; Return a list containing a couple of filled boards:
(check-expect (fill-board 0 TB1) (list TB1a TB1b))
(check-expect (fill-board 0 TB2) (list (cons "X" (rest TB2)) (cons "O" (rest TB2))))
(check-expect (fill-board 1 (cons "O" (rest TB2))) (list TB2c TB2d))

;; Stub:
;; (define (fill-board i board) empty)
(define (fill-board i board)
  (list (fill-square i "X" board)
        (fill-square i "O" board)))



;; Index Value Board -> Board
;; Return a board filled with the value provided at the index provided
(check-expect (fill-square 0 "X" TB1) TB1a)
(check-expect (fill-square 0 "X" TB2) (cons "X" (rest TB2)))
(check-expect (fill-square 1 "X" (cons "X" (rest TB2))) (cons "X" (cons "X" (rest (rest TB2)))))

;; Stub:
;;(define (fill-square i l board) empty)

(define (fill-square i l board)
  (append (take board i)
          (list l)
          (drop board (+ i 1))))

;  PROBLEM 3:
;  
;  Now adapt your solution to filter out the boards that are impossible if
;  X and O are alternating turns. You can continue to assume that they keep
;  filling the board after someone has won though.
;  
;  You can assume X plays first, so all valid boards will have 5 Xs and 4 Os.
;  
;  NOTE: make sure you keep a copy of your solution from problem 2 to answer
;  the questions on edX.
;  


;; (listof Boards) -> (listof Boards)
;; Filter boards that are invalid in the list of Boards generated using
;; the function gen-boards designed in Problem 2
(check-expect (filter-board empty) empty)
(check-expect (filter-board (list TB0)) empty)
(check-expect (filter-board (list TB3)) (list TB3))
(check-expect (filter-board (list TB3 TB4)) (list TB3 TB4))
(check-expect (filter-board (list TB0 TB3 TB4)) (list TB3 TB4))

;; Stub:
;; (define (filter-board lob) empty)

(define (filter-board lob)
  (cond [(empty? lob) empty]
        [else
         (if (board-valid (first lob))
             (cons (first lob) (filter-board (rest lob)))
             (filter-board (rest lob)))]))


;; Board -> Boolean
;; Check if a board is valid. Since it is a full board, and "X" moves first
;; it is valid if it has 5 "X"s and 4 "O"x.
(check-expect (board-valid empty) false)
(check-expect (board-valid TB3) true)
(check-expect (board-valid TB4) true)
(check-expect (board-valid (cons "O" (rest TB4))) false)

;; Stub:
;; (define (board-valid board) false)

(define (board-valid board)
  (local [(define (num-of-x board)
            (length (filter get-x board)))

          (define (get-x value)
            (string=? value "X"))]
    (if (= (num-of-x board) 5)
        true
        false)))
```

### Module 11: Accumulators

#### Sample Problem: skipn

```ISL
;; (listof X) Natural -> (listof X)
(check-expect (skipn empty 2) empty)
(check-expect (skipn (list "a" "b" "c") 2) (list "a"))
(check-expect (skipn (list "a" "b" "c" "d" "e" "f") 2) (list "a" "d"))
(check-expect (skipn (list "a" "b" "c" "d" "e" "f") 1) (list "a" "c" "e"))
(check-expect (skipn (list 1 2 3 4 5 6) 2) (list 1 4))

;; Using design template for accumulator
(define (skipn lox0 n)
  (local [(define (skipn lox n acc)
            (cond [(empty? lox) empty]
                  [else
                   (if (= (remainder acc (add1 n)) 0)
                        (cons (first lox) (skipn (rest lox) n (+ acc 1)))
                        (skipn (rest lox) n
                               (+ acc 1)))]))]

    (skipn lox0 n 0)))
```

* n = 0 is a good test case to include as well


**Note to self**

It may sometimes be helpful to write examples for accumulator as well.

#### Tail Recursion Part 1

* There is a nice explanation involving the stepper in DrRacket for showing a case of stack overflow
* AND a nice example that I assume will lead to TCO, too!

#### Tail Recursion Part 4

* TCO!

#### Worklist Accumulators 1, Part 2

The inconsistent titles with "Part" and "pt" is driving me nuts. Yes, OCD.

```ISL
;; Wizard -> (listof Wizard)
;; Produces the names of every wizard in the tree that was placed in the
;; same house as their immediate parent. (From problem statement)

(check-expect (list-wiz Wa) (list "A"))
(check-expect (list-wiz Wg) (list "G" "A"))
(check-expect (list-wiz Wh) (list "H"))
(check-expect (list-wiz Wk) (list "K" "B"))

(define (list-wiz w)          
  (local [(define house (wiz-house w))

          (define (fn-for-wiz w)
            (if (string=? (wiz-house w) house)
                (cons (wiz-name w) (fn-for-low (wiz-kids w)))
                (fn-for-low (wiz-kids w))))

          (define (fn-for-low low)
            (cond [(empty? low) empty]
                  [else
                   (append (fn-for-wiz (first low))
                           (fn-for-low (rest low)))]))]
    (fn-for-wiz w)))
```

Oops... that was an incorrect attempt that lists all children in the same house as the ancestor input. Not sure what I was thinking just now and completely ignored the problem statement.

Reattempt:

```ISL
;; Wizard -> (listof Wizard)
;; Produces the names of every wizard in the tree that was placed in the
;; same house as their immediate parent. (From problem statement)

(check-expect (list-wiz2 We) empty)
(check-expect (list-wiz2 Wg) (list "A"))
(check-expect (list-wiz2 Wj) (list "E" "F" "A"))
(check-expect (list-wiz2 Wk) (list "E" "F" "A"))

(define (list-wiz2 w)          
  (local [(define (fn-for-wiz w pouse)
            (if (string=? (wiz-house w) pouse)
                (cons (wiz-name w) (fn-for-low (wiz-kids w) (wiz-house w)))
                (fn-for-low (wiz-kids w) (wiz-house w))))

          (define (fn-for-low low pouse)
            (cond [(empty? low) empty]
                  [else
                   (append (fn-for-wiz (first low) pouse)
                           (fn-for-low (rest low) pouse))]))]
    (fn-for-wiz w "")))
```

Urgh, clearly not focusing today. Was told to write the template in the video and jumped to the solution instead.

#### Worklist Accumulators 1, Part 2

* In a tail recursive tree traversal of arbitrary tree, each node is only called once
* Following from above, and depending on the order of parameters used to build the work-list, it's either depth-first or breadth first

#### Recommended Problems

**Accumulators P1 - Drop n**

```ISL
;; (listof X) -> (list of X)
;; Drop ever nth element in (listof X) and return the corresponding list with
;; elements dropped
(check-expect (dropn empty 0) empty)
(check-expect (dropn (list 0 1 2) 0) empty)
(check-expect (dropn (list 0 1 2) 1) (list 0 2))
(check-expect (dropn (list 0 1 2) 2) (list 0 1))
(check-expect (dropn (list 1 2 3 4 5 6 7) 1) (list 1 3 5 7))
(check-expect (dropn (list 1 2 3 4 5 6 7) 2) (list 1 2 4 5 7))

;; Stub:
;; (define (dropn lox n) empty)

;; Template:
#;
(define (fn-for-lox lox n)
  (cond [(empty? lox) ...]
        [else (... (first lox)
                   (fn-for-lox (rest lox) n))]))

(define (dropn lox n)
  (local [(define (inner-dropn lox n index)
            ;; If index is 0, drop everything
            ;; If index is 1 drop every second
            ;; If index is 2 drop every third... etc
            ;; The accumulator index is keeps track of how many elements
            ;; have been retained, when n = index, the current item will
            ;; not be added to the list
            (cond [(or (empty? lox) (= n 0)) empty]
                  [else (if (= index n)
                            (inner-dropn (rest lox) n 0)
                            (cons (first lox) (inner-dropn (rest lox) n (add1 index))))]))]
    (inner-dropn lox n 0)))
```

**Accumulators P4 - Average Tail Recursive**

```ISL
;; (listof Number) -> Number
;; Produces the average of the numbers in the a given list of Number
(check-expect (average empty) 0)
(check-expect (average (list 0)) 0)
(check-expect (average (list 1)) 1)
(check-expect (average (list 2 2)) 2)
(check-expect (average (list 2 2 5)) 3)

;; Stub:
;; (define (average lon) 0)

;; Template:
#;
(define (fn-for-lon lon)
  (cond [(empty? lon) ...]
        [else (... (first lon)
                   (fn-for-lon (rest lon)))]))

(define (average lon)
  (local [(define (fn-for-lon lon sum nums)
            (cond [(and (empty? lon) (= 0 nums)) 0]
                  [(empty? lon) (/ sum nums)]
                  [else (fn-for-lon (rest lon) (+ sum (first lon)) (add1 nums))]))]
    (fn-for-lon lon 0 0)))
```

**Accumulators P12 - Contains Key Tail Recursive**

```ISL
(define-struct node (k v l r))
;; BT is one of:
;;  - false
;;  - (make-node Integer String BT BT)
;; Interp. A binary tree, each node has a key, value and 2 children
(define BT1 false)
(define BT2 (make-node 1 "a"
                       (make-node 6 "f"
                                  (make-node 4 "d" false false)
                                  false)
                       (make-node 7 "g" false false)))


;; Number BT -> Boolean
;; Consumes a key and a binary tree and produces true if the tree contains the key
(check-expect (contains? 1 (make-node 1 "a" false false)) true)
(check-expect (contains? 1 (make-node 2 "b" false false)) false)
(check-expect (contains? 1 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      false)) true)
(check-expect (contains? 2 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      false)) true)
(check-expect (contains? 3 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      false)) false)
(check-expect (contains? 1 (make-node 1 "a"
                                      false
                                      (make-node 2 "b" false false))) true)
(check-expect (contains? 2 (make-node 1 "a"
                                      false
                                      (make-node 2 "b" false false))) true)
(check-expect (contains? 3 (make-node 1 "a"
                                      false
                                      (make-node 2 "b" false false))) false)
(check-expect (contains? 1 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      (make-node 3 "c" false false))) true)
(check-expect (contains? 2 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      (make-node 3 "c" false false))) true)
(check-expect (contains? 3 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      (make-node 3 "c" false false))) true)
(check-expect (contains? 4 (make-node 1 "a"
                                      (make-node 2 "b" false false)
                                      (make-node 3 "c" false false))) false)
(check-expect (contains? 4 (make-node 1 "a"
                                      (make-node 2 "b"
                                                 (make-node 4 "d" false false)
                                                 false)
                                      (make-node 3 "c" false false))) true)

;; Stub:
;; (define (contains? key bt) false)

;; Template:
#;
(define (fn-for-bt bt)
  (... (node-k bt)
       (node-v bt)
       (fn-for-bt (node-l bt))
       (fn-for-bt (node-r bt))))

;; Intermediate:
#;
(define (contains? key bt)
  (local [(define (fn-for-bt key bt)
            (... key
                 (node-k bt)
                 (node-v bt)
                 (fn-for-bt ... (node-l bt))
                 (fn-for-bt ... (node-r bt))))]
    (fn-for-bt key bt)))

(define (contains? key bt)
  (local [(define (fn-for-bt bt lon)
            (if (= (node-k bt) key)
                true
                (fn-for-lon (append lon (valid-children (node-l bt) (node-r bt))))))

          (define (fn-for-lon lon)
            (cond [(empty? lon) false]
                  [else (fn-for-bt (first lon) (rest lon))]))

          (define (valid-children l r)
            (cond [(and (false? l) (false? r)) empty]
                  [(false? l) (list r)]
                  [(false? r) (list l)]
                  [else (list l r)]))]
    (fn-for-bt bt empty)))
```

#### Quiz

Problem 1:

```ISL
;; (listof String) -> Natural
;; Get the length of the longest string in a list of string

;; Examples:
(check-expect (longest-string-length empty) 0)
(check-expect (longest-string-length (list "a")) 1)
(check-expect (longest-string-length (list "a" "ab")) 2)
(check-expect (longest-string-length (list "ab" "a")) 2)
(check-expect (longest-string-length (list "a" "ab" "abc")) 3)
(check-expect (longest-string-length (list "1" "12" "123")) 3)

;; Stub:
;; (define (longest-string-length str) 0)

;; Template:
#;
(define (fn-for-los los)
  (cond [(empty? los) ...]
        [else (... (first los)
                   (fn-for-los (rest los)))]))

(define (longest-string-length los)
  (local [(define (fn-for-los los acc)
            (cond [(empty? los) acc]
                  [else (fn-for-los (rest los)
                                    (max (string-length (first los)) acc))]))]
    (fn-for-los los 0)))
```

Problem 2:

```ISL
;; (listof Natural) -> Boolean
;; Determines if the list obeys the fibonacci rule, n-2 + n-1 = n. If so,
;; return true, else return false.

;; Examples:
(check-expect (valid-fib (list 0 1)) true)
(check-expect (valid-fib (list 1 3)) true)
(check-expect (valid-fib (list 0 1 1 2)) true)
(check-expect (valid-fib (list 0 1 1 3)) false)
(check-expect (valid-fib (list 2 3 5 8)) true)
(check-expect (valid-fib (list 2 3 5 9)) false)

;; Stub:
;; (define (valid-fib lon) false)

;; Template:
#;
(define (fn-for-lon lon)
  (cond [(empty? lon) ...]
        [else (... (first lon)
                   (fn-for-lon (rest lon)))]))

(define (valid-fib lon)
  (local [(define m-init (first lon))
          (define j-init (first (rest lon)))
          (define lon-init (rest (rest lon)))

          (define (fn-for-lon lon m n)
            (cond [(empty? lon) true]
                  [else (if (= (first lon) (+ m n))
                            (fn-for-lon (rest lon) n (first lon))
                            false)]))]

    (fn-for-lon lon-init m-init j-init)))
```

Problem 3:

```ISL
;; Natural -> Natural
;; produces the factorial of the given number
(check-expect (fact 0) 1)
(check-expect (fact 3) 6)
(check-expect (fact 5) 120)

#;
(define (fact n)
  (cond [(zero? n) 1]
        [else
         (* n (fact (sub1 n)))]))

(define (fact n)
  (local [(define (local-fact n acc)
            (cond [(zero? n) acc]
                  [else (local-fact (sub1 n) (* acc n))]))]
    (local-fact n 1)))
```

Problem 4:

```ISL
;; Region -> Natural
;; Produces the number of regions within and including a given Region
(check-expect (count-regions VANCOUVER) 1)
(check-expect (count-regions BC) 3)
(check-expect (count-regions CANADA) 7)

;; Stub:
;; (define (count-regions region) 1)

(define (count-regions r)
  (local [(define (fn-for-region r c todo)
            (fn-for-lor (append todo (region-subregions r)) (add1 c)))

          (define (fn-for-lor todo c)
            (cond [(empty? todo) c]
                  [else (fn-for-region (first todo) c (rest todo))]))]
    (fn-for-region r 0 empty)))
```  

### Module 12: Graphs

* Acyclic graphs—graphs with no cycles

```ASL
(shared ((-0- (make-room "A" (list (make-room "B" (list (make-room "C" (list -0-))))))) -0-))
```

#### Recommended Problems

**Graphs P2 - Count Rooms**

```ASL
;; Room -> Natural
;; Produces the total number of rooms reachable from the given room.
;; Include the starting room itself.
(check-expect (count-rooms (make-room "B" empty)) 1)
(check-expect (count-rooms H1) 2)
(check-expect (count-rooms H2) 2)
(check-expect (count-rooms H3) 3)
(check-expect (count-rooms H4) 6)
(check-expect (count-rooms (first (rest (room-exits H4)))) 6)

;; Stub:
;; (define (count-rooms r) 1)

(define (count-rooms r0)
  ;; todo is (listof Room); a worklist accumulator
  ;; visited is (listof String); context preserving accumulator, names of rooms already visited
  ;; n is Natural; the number of unique rooms that have been visited so far
  (local [(define (fn-for-room r todo visited n)
            (if (member (room-name r) visited)
                (fn-for-lor todo visited n)
                (fn-for-lor (append (room-exits r) todo)
                            (cons (room-name r) visited)
                            (add1 n)))) ; (... (room-name r))

          (define (fn-for-lor todo visited n)
            (cond [(empty? todo) n]
                  [else
                   (fn-for-room (first todo)
                                (rest todo)
                                visited
                                n)]))]

    (fn-for-room r0 empty empty 0)))
```

**Graphs P4 - Max Exits From**

```ASL
;; Room -> Room
;; Produces the room with the most exits (in the case of a
;; tie you can produce any of the rooms in the tie)
(check-expect (max-exits (make-room "N" empty)) (make-room "N" empty))
(check-expect (max-exits H1) H1)
(check-expect (max-exits H3) H3)
(check-expect (max-exits H4) H4)
(check-expect (max-exits (shared ((-A- (make-room "A" (list -B-)))
                                  (-B- (make-room "B" (list -C-)))
                                  (-C- (make-room "C" (list -A-))))
                           -B-))
              (shared ((-A- (make-room "A" (list -B-)))
                       (-B- (make-room "B" (list -C-)))
                       (-C- (make-room "C" (list -A-))))
                -B-))

;; Stub:
;; (define (max-exits room) (make-room "N" empty))


(define (max-exits r0)
  ;; todo is (listof Room); a worklist accumulator
  ;; visited is (listof String); context preserving accumulator, names of rooms already visited
  ;; rmax is Room; the room with the most exits of all the rooms that have been visited so far
  (local [(define (fn-for-room r todo visited rmax)
            (if (member (room-name r) visited)
                (fn-for-lor todo visited rmax)
                (fn-for-lor (append (room-exits r) todo)
                            (cons (room-name r) visited)
                            (more-exits rmax r)))) ; (... (room-name r))

          (define (fn-for-lor todo visited rmax)
            (cond [(empty? todo) rmax]
                  [else
                   (fn-for-room (first todo)
                                (rest todo)
                                visited
                                rmax)]))

          (define (more-exits r1 r2) ; Room Room -> Room
            (if (< (length (room-exits r1)) (length (room-exits r2)))
                r2
                r1))]

    (fn-for-room r0 empty empty r0)))
```

**Graphs P5 - Max Exits To**

```ASL

;; Room -> Room
;; Produces the room to which the greatest number of other rooms
;; have exits (in the case of a tie you can produce any of the rooms in the tie)
;; This is depth-first search—the result is particularly apparent for the case
;; of H3. For H4, the result depends on how far the node with the most exits
;; is away from the starting node.
(check-expect (max-exits-to (make-room "N" empty)) (make-room "N" empty))
(check-expect (max-exits-to H1) (first (room-exits H1)))
(check-expect (max-exits-to H2) (first (room-exits H2)))
(check-expect (max-exits-to (first (room-exits H2))) H2)
(check-expect (max-exits-to H3) (first (room-exits (first (room-exits H3)))))
(check-expect (max-exits-to (first (room-exits (first (room-exits H3)))))
              (first (room-exits H3)))
(check-expect (max-exits-to H4) (shared ((-A- (make-room "A" (list -B- -D-)))
                                         (-B- (make-room "B" (list -C- -E-)))
                                         (-C- (make-room "C" (list -B-)))
                                         (-D- (make-room "D" (list -E-)))
                                         (-E- (make-room "E" (list -F- -A-)))
                                         (-F- (make-room "F" (list))))
                                  -E-))
(check-expect (max-exits-to (shared ((-A- (make-room "A" (list -B- -D-)))
                                     (-B- (make-room "B" (list -C- -E-)))
                                     (-C- (make-room "C" (list -B-)))
                                     (-D- (make-room "D" (list -E-)))
                                     (-E- (make-room "E" (list -F- -A-)))
                                     (-F- (make-room "F" (list))))
                              -D-))
              (shared ((-A- (make-room "A" (list -B- -D-)))
                       (-B- (make-room "B" (list -C- -E-)))
                       (-C- (make-room "C" (list -B-)))
                       (-D- (make-room "D" (list -E-)))
                       (-E- (make-room "E" (list -F- -A-)))
                       (-F- (make-room "F" (list))))
                -B-))
(check-expect (max-exits-to (shared ((-A- (make-room "A" (list -B- -D-)))
                                     (-B- (make-room "B" (list -C- -E-)))
                                     (-C- (make-room "C" (list -B-)))
                                     (-D- (make-room "D" (list -E-)))
                                     (-E- (make-room "E" (list -F- -A-)))
                                     (-F- (make-room "F" (list))))
                              -C-))
              (shared ((-A- (make-room "A" (list -B- -D-)))
                       (-B- (make-room "B" (list -C- -E-)))
                       (-C- (make-room "C" (list -B-)))
                       (-D- (make-room "D" (list -E-)))
                       (-E- (make-room "E" (list -F- -A-)))
                       (-F- (make-room "F" (list))))
                -E-))

;; Stub
;; (define (max-exits-to room) (make-room "N" empty))

(define (max-exits-to r0)
  ;; todo is (listof Room); a worklist accumulator
  ;; visited is (listof String); context preserving accumulator, names of rooms already visited
  (local [(define (fn-for-room r todo visited rooms-entered)
            (if (member r visited)
                (fn-for-lor todo visited rooms-entered)
                (fn-for-lor (append (room-exits r) todo)
                            (cons r visited)
                            (append rooms-entered (room-exits r)))))

          (define (fn-for-lor todo visited rooms-entered)
            (cond [(empty? todo) (filter-room visited rooms-entered)]
                  [else
                   (fn-for-room (first todo)
                                (rest todo)
                                visited
                                rooms-entered)]))]
    (fn-for-room r0 empty empty empty)))


;; Room -> Room
;; Return the room that has been visited (most exits) the most
(check-expect (filter-room (list (make-room "N" empty)) (list (make-room "N" empty))) (make-room "N" empty))
(check-expect (filter-room (list (make-room "N" empty) (make-room "A" empty))
                           (list (make-room "N" empty) (make-room "A" empty)))
              (make-room "N" empty))
(check-expect (filter-room (list (make-room "N" empty) (make-room "A" empty))
                           (list (make-room "N" empty) (make-room "N" empty) (make-room "A" empty)))
              (make-room "N" empty))
(check-expect (filter-room (list (make-room "N" empty) (make-room "A" empty))
                           (list (make-room "N" empty) (make-room "A" empty) (make-room "A" empty)))
              (make-room "A" empty))

;; Stub:
;; (define (filter-room visited rooms-entered) (make-room "N" empty))

(define (filter-room visited rooms-entered)
  ;; todo is (listof Room); a worklist accumulator
  ;; visited is (listof String); context preserving accumulator, names of rooms already visited
  (local [(define (fn-for-room room uncounted max rmax)
            (if (> (get-count room) max)
                (fn-for-lor uncounted (get-count room) room)
                (fn-for-lor uncounted max rmax)))

          (define (fn-for-lor uncounted max rmax)
            (cond [(empty? uncounted) rmax]
                  [else
                   (fn-for-room (first uncounted)
                                (rest uncounted)
                                max
                                rmax)]))

          (define (get-count room)
            (length (filter (lambda (r) (string=? (room-name r) (room-name room))) rooms-entered)))]

    (fn-for-room (first visited) (rest visited) 0 (first visited))))
```

### Final Project

#### Part 1

Problem 1:

```ASL
(require racket/list)


;  PROBLEM 1:
;  
;  Consider a social network similar to Twitter called Chirper. Each user has a name, a note about
;  whether or not they are a verified user, and follows some number of people.
;  
;  Design a data definition for Chirper, including a template that is tail recursive and avoids
;  cycles.
;  
;  Then design a function called most-followers which determines which user in a Chirper Network is
;  followed by the most people.
;  


;; ================
;; Data Definitions
;; ================

(define-struct user (name verified following))
;; User is (make-user String Boolean (listof User))
;; interp. String is a user's name, Boolean indicates whether or not a user is verified, and
;; (listof User) is a list of other uers that a given user follows.

;; Images for graphs borrowed from max-exits-to-starter.rkt from recommended problems.
; .

(define C1 (shared ((-A- (make-user "A" true (list -B-)))
                    (-B- (make-user "B" true (list))))
             -A-))

; .

(define C2 (shared ((-A- (make-user "A" true (list -B-)))
                    (-B- (make-user "B" true (list -A-))))
             -A-))

; .

(define C3 (shared ((-A- (make-user "A" true (list -B-)))
                    (-B- (make-user "B" true (list -C-)))
                    (-C- (make-user "C" true (list -A-))))
             -A-))

; .

(define C4
  (shared ((-A- (make-user "A" true (list -B- -D-)))
           (-B- (make-user "B" true (list -C- -E-)))
           (-C- (make-user "C" true (list -B-)))
           (-D- (make-user "D" true (list -E-)))
           (-E- (make-user "E" true (list -F- -A-)))
           (-F- (make-user "F" true (list))))
    -A-))

;; Template:
;; Structural recursion, tail recursive, avoids cycles, accumulator for user with the most followers

;; Base template based on data definitions:
#;
(define (fn-for-chirper user)
  (local [(define (fn-for-user user)
            (... (user-name user)
                 (user-verified user)
                 (fn-for-lou (user-following user)))) ;; Reference rule

          (define (fn-for-lou lou)
            (cond [(empty? lou) (...)]
                  [else
                   (... (fn-for-user (first lou))
                        (fn-for-lou (rest lou)))]))]

    (fn-for-user user)))

;; Make tail-recursive
(define (fn-for-chirper user)
  (local [(define (fn-for-user user todo visited)
            (if (member user visited)
                (fn-for-lou todo
                            visited) ;; Nothing needs to be done if algorithm has already been here
                (fn-for-lou (append todo (user-following user)) ;; Otherwise add list of child users to todo
                            (append visited user)))) ;; And add the current user to visited

          (define (fn-for-lou todo visited)
            (cond [(empty? todo) (...)]
                  [else (fn-for-user (first todo)
                                     (rest todo)
                                     visited)]))]

    (fn-for-user user empty empty)))


;; User -> User
;; Given a user determine which user in the network has the most followers
;; If there is more than one user with the same amount of max followers, the first
;; user that the algorithm comes across will be returned
(check-expect (most-followers C1) (first (user-following C1)))
(check-expect (most-followers (first (user-following C1))) (first (user-following C1)))
(check-expect (most-followers C2) C2)
(check-expect (most-followers (first (user-following C2))) (first (user-following C2)))
(check-expect (most-followers C3) C3)
(check-expect (most-followers (first (user-following (first (user-following C3))))) (first (user-following (first (user-following C3)))))
(check-expect (most-followers C4) (shared ((-A- (make-user "A" true (list -B- -D-)))
                                           (-B- (make-user "B" true (list -C- -E-)))
                                           (-C- (make-user "C" true (list -B-)))
                                           (-D- (make-user "D" true (list -E-)))
                                           (-E- (make-user "E" true (list -F- -A-)))
                                           (-F- (make-user "F" true (list))))
                                    -B-))
(check-expect (most-followers (shared ((-A- (make-user "A" true (list -B- -D-)))
                                       (-B- (make-user "B" true (list -C- -E-)))
                                       (-C- (make-user "C" true (list -B-)))
                                       (-D- (make-user "D" true (list -E-)))
                                       (-E- (make-user "E" true (list -F- -A-)))
                                       (-F- (make-user "F" true (list))))
                                -D-))
              (shared ((-A- (make-user "A" true (list -B- -D-)))
                       (-B- (make-user "B" true (list -C- -E-)))
                       (-C- (make-user "C" true (list -B-)))
                       (-D- (make-user "D" true (list -E-)))
                       (-E- (make-user "E" true (list -F- -A-)))
                       (-F- (make-user "F" true (list))))
                -E-))

;; Stub:
;; (define (most-followers user) C1)

;; Using template from above:
(define (most-followers user)
  (local [(define (fn-for-user user todo visited followed)
            (if (member user visited)
                (fn-for-lou todo
                            visited
                            followed)
                (fn-for-lou (append todo (user-following user))
                            (append visited (list user))
                            followed)))

          (define (fn-for-lou todo visited followed)
            (cond [(empty? todo) (most-followed visited followed)]
                  [else (fn-for-user (first todo)
                                     (rest todo)
                                     visited
                                     (cons (first todo) followed))]))

          (define (most-followed visited followed)
            (list-ref visited (index-of-max (get-max visited followed))))

          (define (get-max visited followed)
            (map (lambda (user)
                   (length
                    (filter (lambda (follow) (string=? (user-name user) (user-name follow)))
                            followed)))
                 visited))

          (define (index-of-max lon)
            (index-of lon (find-max lon 0)))

          (define (find-max lon acc)
            (cond [(empty? lon) acc]
                  [else (if (> (first lon) acc)
                            (find-max (rest lon) (first lon))
                            (find-max (rest lon) acc))]))]

    (fn-for-user user empty empty empty)))
```

#### Part 2:

```ASL
;; Slot is Natural
;; interp. each TA slot has a number, is the same length, and none overlap

(define-struct ta (name max avail))
;; TA is (make-ta String Natural (listof Slot))
;; interp. the TA's name, number of slots they can work, and slots they're available for

(define SOBA (make-ta "Soba" 2 (list 1 3)))
(define UDON (make-ta "Udon" 1 (list 3 4)))
(define RAMEN (make-ta "Ramen" 1 (list 2)))
(define SOMEN (make-ta "Somen" 3 (list 1 2 3)))
(define TSUKEMEN (make-ta "Tsukemen" 1 (list 5)))

(define NOODLE-TAs (list SOBA UDON RAMEN))

(define ERIKA (make-ta "Erika" 1 (list 1 3 7 9)))
(define RYAN (make-ta "Ryan" 1 (list 1 8 10)))
(define REECE (make-ta "Reece" 1 (list 5 6)))
(define GORDON (make-ta "Gordon" 2 (list 2 3 9)))
(define DAVID (make-ta "David" 2 (list 2 8 9)))
(define KATIE (make-ta "Katie" 1 (list 4 6)))
(define AASHISH (make-ta "Aashish" 2 (list 1 10)))
(define GRANT (make-ta "Grant" 2 (list 1 11)))
(define RAEANNE (make-ta "Raeanne" 2 (list 1 11 12)))

(define PROJECT-TAs (list ERIKA RYAN REECE GORDON DAVID KATIE AASHISH GRANT RAEANNE))


(define-struct assignment (ta slot))
;; Assignment is (make-assignment TA Slot)
;; interp. the TA is assigned to work the slot

;; Schedule is (listof Assignment)


;; ============================= FUNCTIONS


;; (listof TA) (listof Slot) -> (listof Assignment) or false
;; produce valid schedule given TAs and Slots; false if impossible

(check-expect (schedule-tas empty empty) empty)
(check-expect (schedule-tas empty (list 1 2)) false)
(check-expect (schedule-tas (list SOBA) empty) empty)

(check-expect (schedule-tas (list SOBA) (list 1)) (list (make-assignment SOBA 1)))
(check-expect (schedule-tas (list SOBA) (list 2)) false)
(check-expect (schedule-tas (list SOBA) (list 1 3)) (list (make-assignment SOBA 1)
                                                          (make-assignment SOBA 3)))

(check-expect (schedule-tas NOODLE-TAs (list 1 3 4))
              (list
               (make-assignment SOBA 1)
               (make-assignment SOBA 3)
               (make-assignment UDON 4)))

(check-expect (schedule-tas NOODLE-TAs (list 1 2 3 4))
              (list
               (make-assignment SOBA 1)
               (make-assignment RAMEN 2)
               (make-assignment SOBA 3)
               (make-assignment UDON 4)))

(check-expect (schedule-tas NOODLE-TAs (list 1 2 3 4 5)) false)
(check-expect (schedule-tas (cons TSUKEMEN NOODLE-TAs) (list 1 2 3 4 5))
              (list
               (make-assignment SOBA 1)
               (make-assignment RAMEN 2)
               (make-assignment SOBA 3)
               (make-assignment UDON 4)
               (make-assignment TSUKEMEN 5)))

(check-expect (schedule-tas (list ERIKA RYAN REECE GORDON DAVID KATIE AASHISH GRANT RAEANNE) (list 1 2 4 3 5 6)) false)
(check-expect (schedule-tas (list ERIKA RYAN REECE GORDON DAVID KATIE AASHISH GRANT RAEANNE) (list 1 2 3 5 6))
              (list
               (make-assignment RAEANNE 1)
               (make-assignment DAVID 2)
               (make-assignment GORDON 3)
               (make-assignment REECE 5)
               (make-assignment KATIE 6)))
(check-expect (schedule-tas (list ERIKA RYAN REECE GORDON DAVID KATIE AASHISH GRANT RAEANNE) (list 1 2 3 4 6))
              (list
               (make-assignment RAEANNE 1)
               (make-assignment DAVID 2)
               (make-assignment GORDON 3)
               (make-assignment KATIE 4)
               (make-assignment REECE 6)))

;; Stub:
;; (define (schedule-tas tas slots) empty)

;; After drawing a few trees on paper, it appears that the problem appears
;; is likely solvable using arbitrary tree, recursive generation
;; and backtracking search.

;; Using template from the design recipes:
(define (schedule-tas tas slots)
  (local [(define (solve--slots tas slots)
            (cond [(solved? slots) slots]
                  [(empty? tas) false]
                  [else (solve--loslots (rest tas) (cons slots (next-loslots (first tas) slots)))])) ;; (cons slots ...) includes slots or otherwise the algorithm won't be able to backtrack to the current slot

          (define (solve--loslots tas loslots)
            (cond [(empty? loslots) false]
                  [else
                   (local [(define try (solve--slots tas (first loslots)))]
                     (if (not (false? try))
                         try
                         (solve--loslots tas (rest loslots))))]))]

    (solve--slots tas slots)))


;; (listof Slot) -> Boolean
;; Determines whether or not a list of Slot is a valid Schedule
(check-expect (solved? empty) true)
(check-expect (solved? (list 1)) false)
(check-expect (solved? (list 1 2 3)) false)
(check-expect (solved? (list (make-assignment SOBA 1))) true)
(check-expect (solved? (list (make-assignment SOBA 1) 2)) false)
(check-expect (solved? (list (make-assignment SOBA 1) (make-assignment RAMEN 2))) true)
(check-expect (solved? (list (make-assignment SOBA 1) 2 3)) false)
(check-expect (solved? (list (make-assignment SOBA 1) (make-assignment RAMEN 2) 3)) false)
(check-expect (solved? (list (make-assignment SOBA 1) (make-assignment RAMEN 2) (make-assignment UDON 3))) true)

;; Stub:
;; (define (solved? slots) false)

(define (solved? slots)
  (if (empty? slots)
      true
      (= (length (filter number? slots)) 0)))

;; TA (listof Slot) -> (listof (listof Slot))
;; Generate the next possibe slots for a given TA
(check-expect (next-loslots SOBA empty) empty)

(check-expect (next-loslots SOBA (list 2)) empty)
(check-expect (next-loslots SOBA (list 1)) (list (list (make-assignment SOBA 1))))
(check-expect (next-loslots RAMEN (list 2)) (list (list (make-assignment RAMEN 2))))
(check-expect (next-loslots SOBA (list 3)) (list (list (make-assignment SOBA 3))))

(check-expect (next-loslots SOBA (list (make-assignment SOBA 1) 3))
              (list (list (make-assignment SOBA 1) (make-assignment SOBA 3))))
(check-expect (next-loslots SOBA (list 1 3))
              (list (list (make-assignment SOBA 1) 3)
                    (list 1 (make-assignment SOBA 3))
                    (list (make-assignment SOBA 1) (make-assignment SOBA 3))))
(check-expect (next-loslots SOMEN (list 1 2 3))
              (list (list (make-assignment SOMEN 1) 2 3)
                    (list 1 (make-assignment SOMEN 2) 3)
                    (list 1 2 (make-assignment SOMEN 3))
                    (list 1 (make-assignment SOMEN 2) (make-assignment SOMEN 3))
                    (list (make-assignment SOMEN 1) (make-assignment SOMEN 2) 3)
                    (list (make-assignment SOMEN 1) 2 (make-assignment SOMEN 3))
                    (list (make-assignment SOMEN 1) (make-assignment SOMEN 2) (make-assignment SOMEN 3))))

;; Stub:
;; (define (next-loslots ta slots) empty)

(define (next-loslots ta slots)
  (local [(define orig-ta ta)

          (define (gen ta slots)
            (cond [(zero? (ta-max ta)) empty]
                  [(empty? (ta-avail ta)) empty]
                  [else (local [(define fav (first (ta-avail ta)))
                                (define rav (rest (ta-avail ta)))
                                (define filled (fill-one-slot fav slots))]
                          (append filled
                                  (gen (make-ta (ta-name ta)
                                                (ta-max ta)
                                                rav)
                                       slots)
                                  (if (empty? filled)
                                      empty
                                      (gen (make-ta (ta-name ta)
                                                    (sub1 (ta-max ta))
                                                    (ta-avail ta))
                                           (first filled)))))]))

          (define (fill-one-slot slot slots)
            (local [(define i (index-of slots slot))]
              (if (false? i)
                  empty
                  (list (append (take slots i)
                                (list (make-assignment orig-ta slot))
                                (drop slots (add1 i)))))))

          (define (sub-max ta)
            (make-ta (ta-name ta) (sub1 (ta-max ta)) (ta-avail ta)))]

    (gen ta slots)))


(schedule-tas PROJECT-TAs (list 1 2 3 4 5 6 7 8 9 10 11 12))
(schedule-tas (cons (make-ta "Alex" 1 (list 7)) PROJECT-TAs) (list 1 2 3 4 5 6 7 8 9 10 11 12))
(schedule-tas (cons (make-ta "Erin" 1 (list 4)) PROJECT-TAs) (list 1 2 3 4 5 6 7 8 9 10 11 12))
```


## Resources

* Worklist Accumulators 1, Part 2 is particularly worth revisiting for those who have been following the curriculum. I personally think that it's a very good example of depth-first and breadth-first search.
