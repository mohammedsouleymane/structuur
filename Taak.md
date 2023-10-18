## Golomb getallen
```scheme
(define (golomb n) (if (= n 1) 1 (+ 1 (golomb (- n (golomb (golomb (- n 1)) ))))) )
```

## Golomb reeks
```scheme
(define (golomb n) (if (= n 1) 1 (+ 1 (golomb (- n (golomb (golomb (- n 1)) ))))) )

(define (golomb-reeks n) (if (= n 1)
                             (display 1)
                             ( begin
                                (golomb-reeks (- n 1))
                                (display " ")
                                (display (golomb n)))))
```

## Binomiaal
```scheme
(define (binom n k)
  (cond
    ((> k n) 0)
    ((or (= k n) (= 0 k )) 1)
    (else (+ (binom (- n 1) (- k 1)) (binom (- n 1) k)))
    )
  )
; recursieve proces
; uw return waarde wordt gebruikt om de waardes bij uw optelling
```

## Driehoek
```scheme
(define (binom n k)
  (cond
    ((> k n) 0)
    ((or (= k n) (= 0 k )) 1)
    (else (+ (binom (- n 1) (- k 1)) (binom (- n 1) k)))
    )
  )
; recursieve proces
; uw return waarde wordt gebruikt om de waardes bij uw optelling


(define (pascal-driehoek n)
  (define (iter x)
    (if ( <= x n )(begin (display-row x) (iter (+ 1 x))))
    )
  (iter 0))

(define (display-row n)
  (define (iter x)
    (if (= n x) (begin (display 1)(newline))
        (begin (display (binom n x))(display "\t") (iter (+ 1 x)))))
  (iter 0))
```

