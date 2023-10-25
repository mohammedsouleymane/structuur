## Opbrengst
```scheme
;iterative
(define (opbrengst start interest jaar)
  (if (= jaar 0)
      start
      (opbrengst (* start (/ (+ 100 interest) 100.0)) interest (- jaar 1)))) 
;start keep updating, if interest = 3 -> start * 1.03 

;recursive
(define (opbrengst start interest jaar)
  (if (= jaar 0)
      start
      (* (/ (+ 100 interest) 100.0) (rec start interest (- jaar 1)))))
```

## Aantal positief
```scheme
(define (aantal-positief lst)
  (cond ((null? lst) 0)
        (( > (car lst) -1) (+ 1 (aantal-positief (cdr lst))))
        (else (aantal-positief (cdr lst)))))

(aantal-positief '(-1 1 2 -2))
(aantal-positief '(0 -1))

;hogere orde
(define (aantal test lst)
  (cond ((null? lst) 0)
        ((test (car lst)) (+ 1 (aantal test (cdr lst))))
        (else (aantal test (cdr lst)))))

(aantal (lambda(x) (> x -1))  '(-1 1 2 -2))
(aantal (lambda(x) (> x -1))  '(0 -1))
```

## Schrap-3
```scheme
(define (schrap-3 lst)
  (define (iter lst n)
    (cond ((null? lst) '())
          ((= n 3) (iter (cdr lst) 1))
          (else (cons (car lst) (iter (cdr lst) (+ 1 n))))
    ))
    (iter lst 1))

(schrap-3 '(1 2 3 4 5 6 7 8))
(schrap-3 '(a b c d e f g h j k l m n o p))


; dynamic
(define (schrap-n n lst)
  (define (iter lst i)
    (cond ((null? lst) '())
          ((= n i) (iter (cdr lst) 1))
          (else (cons (car lst) (iter (cdr lst) (+ 1 i)))) ))
    (iter lst 1))

(schrap-n 4 '(1 2 3 4 5 6 7 8))
(schrap-n 4 '(a b c d e f g h j k l m n o p))
```

## recursie
```scheme
; recursie 4
(define (controleer lijst test)
  (if (null? lijst)
      #t
      (and (test (car lijst))
           (controleer (cdr lijst)
                       test))))
;(a) Wat doet de functie controleer?
; kijkt of alle elementen in de lijst aan een bepaalt test voldoen

;(b) Is de functie controleer een hogere orde functie? Waarom wel/niet?
; wel: neemt een procedure aan test

;(c) Schets de output die je ziet bij de evaluatie van onderstaande oproep van controleer.
 ;(trace controleer)
 ;(controleer '(5 7 1 4 6 4 9) odd?)
; (controleer '(5 7 1 4 6 4 9) odd?)
; (controleer '(7 1 4 6 4 9) odd?)
; (controleer '(1 4 6 4 9) odd?)
; (controleer '(4 6 4 9) odd?)
; #f
```
