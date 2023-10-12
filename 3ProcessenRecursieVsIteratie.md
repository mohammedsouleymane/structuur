## 3.1.1 Recursief
```scheme
(define (1- x) (- x 1))
(define (1+ x) (+ 1 x))

(define (rec-add x y)
 (if (= y 0)
     x
    (+ 1 (rec-add x (1- y)))
   
  ))
```

## 3.1.2 Iteratief

```scheme
(define (1- x) (- x 1))
(define (1+ x) (+ 1 x))

(define (iter-add x y)
 (if (= y 0)
     x
    (iter-add (1+ x) (1- y))
   
  ))
```

## 3.2 Multiply
```scheme
(define (1- x) (- x 1))

(define (rec-multiply x y)

  (if (= y 0) 0
     (+ x (rec-multiply x (1- y))))
  )

(define (iter-multiply x y)
  (define (iter y z) (if (= y 0) z (iter (1- y) (+ z x))))
  (iter y 0)
)
```

## 3.2.1 Fast Multiply
```scheme
(define (1- x) (- x 1))
(define (1+ x) (+ x 1))
(define (double x) (+ x x))
(define (halve x) (/ x 2))
(define (even? x) (= 0 (modulo x 2 )))


;recursive 
(define (rec-fast-multiply x y)
  (cond ((= y 0) 0)
        ((even? y) (double (rec-fast-multiply x (halve y)))  )
        (else (+ x (rec-fast-multiply x (1- y))))
   )
)
;iterative
(define (iter-fast-multiply x y)
  (define (iter ny res) 
    (cond ((= y ny) res) 
          ((<= y (double ny)) (iter (1+ ny) (+ res x)))
          ((> y (double ny)) (iter (double ny) (double res)))
    )
  )
  (if (= y 0) 0 (iter 1 x))
  )
```

## 3.3.1 Het getal e
```scheme
(define (factorial n)
  (if (zero? n)
      1
      (* n (factorial (- n 1)))))



(define (calc-e n)
   (define (iter a res  prev-fac)
     (if (> a n)
         res
         (iter (+ a 1)(+ res (/ 1 (* a prev-fac))) (* a prev-fac)))
     )
  (iter 1 1 1)
  )
```

## 3.5 Mutuele recursie
```scheme
(define (my-even? n)
(if (= n 0)
    #t
    (my-odd? (- n 1))
  ))

(define (my-odd? n)
(if (= 0 n)
    #f
    (my-even? (- n 1))
  ))
```

## 3.6 Diepte van een recursief proces
```scheme
(define (weird n)
(cond ((= n 1) 1)
      ((even? n) (weird (/ n 2)))
      (else (weird (+ 1 (* 3 n)))))
  )
```

## 3.6.2 Bereken de recursie diepte
```scheme
(define (depth-weird n)
  
  (define (weird n d)
    (cond ((= n 1) d)
     ((even? n) (weird (/ n 2) (+ 1 d)))
      (else (weird (+ 1 (* 3 n)) (+ 1 d)))
  ))
  (weird n 0)
  )
```

## 3.7.2 Binaire vormen
```scheme
(define (display-as-binary n)

  (if (>= n 2) (display-as-binary (quotient n 2)))

  (if (even? n) (display "0") (display "1"))
                       
  )
```

## 3.9.1 display-n
```scheme
(define (display-n x n)

  (if (> n 0) (begin (display-n x (- n 1)) (display x)) )
  )
```

## 3.9.2 parasol
```scheme
(define (display-n x n)

  (if (> n 0) (begin (display-n x (- n 1)) (display x)) )
  )

(define (parasol n)
 
  (define (iter x )
    (if (<= x n) (begin
                   (display-n " " (- n x))
                   (display-n "*" (- (* 2 x) 1))
                   (newline)
                   (iter (+ x 1)))
        )
    )
  (iter 1)
  (do ((i 0 (+ i 1)))
    ((> i 2))
  (display-n " " (- n 1))
  (display "*")
  (newline)
  )
  
)
```
