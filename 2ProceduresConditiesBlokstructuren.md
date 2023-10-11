  2.1.1 Sign
```scheme
(define (sign x)
 (cond ((> x 0) 1)
       ((< x 0) -1)
       (else 0)
  ))
```

2.1.2 leap-year?
```scheme
(define (divides? n m)
  (= (modulo n m) 0)
  )
(define (leap-year? x)
 (cond ((divides? x 400) #t)
       ((divides? x 100) #f)
       ((divides? x 4) #t)
       (else #f)
  ))
```

2.3.1 my-and
```scheme
(define (my-and x y)
  (if x y x)
  )
```

2.3.3 my-or
```scheme
(define (my-or x y)
  (if x #t y)
  )
```

2.3.4 my-if
```scheme
(define (my-if x y z)
  (cond (x y)
        (else z)))
```