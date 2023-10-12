## 1.3.1 fourth
```scheme
(define (square x) (* x x))
(define (fourth-* x) (* x x x x))
(define (fourth-square x) (* (square x) (square x)))
```

## 1.3.2 sum-3-squares
```scheme
(define (square x) (* x x))
(define (sum-3-squares x y z) (+ (square x) (square y) (square z) ))
```

## 1.3.3 Celsius naar Fahrenheit
```scheme
(define (convert-C-to-F c) (- (* (+ c 40) 1.8) 40))
```

## 1.8.1 Discount
```scheme
(define (discount p k)(/ (* (- 100 k) p) 100.0))
```

