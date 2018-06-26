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











## Resources
