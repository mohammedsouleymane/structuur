## 6.1.1 Punt
```scheme
(define make-punt cons)
(define x car)
(define y cdr)
```

## 6.1.2 Lijnstuk
```scheme
(define make-punt cons)
(define x car)
(define y cdr)
(define make-segment cons)
(define start-punt car)
(define end-punt cdr)
```

## 6.1.3 MIddelpunt
```scheme
(define make-punt cons)
(define x car)
(define y cdr)
(define make-segment cons)
(define start-punt car)
(define end-punt cdr)

(define (middelpunt s) (make-punt (/ (+ (x (start-punt s)) (x (end-punt s))) 2)  (/ (+ (y (start-punt s)) (y (end-punt s))) 2)))
```
