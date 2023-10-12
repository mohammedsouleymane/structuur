## 4.7.1 Contructieve recursie
```scheme
(define (product factor a next b)
  (if (> a b)
      1
      (* (factor a) (product factor (next a) next b))))
```

## 4.7.2 Staartrecursie
```scheme
(define (iter-product factor a next b)
  (define (iter a res)  (if (> a b) res (iter (next a) (* (factor a) res))))
  (iter a 1))
```

## 4.7.3 Factorial
```scheme
(define (product factor a next b)
  (if (> a b)
      1
      (* (factor a) (product factor (next a) next b))))


(define (factorial n)
  (product + 1 (lambda (x) (+ x 1)) n ))
```

## 4.9.1 Accumulate
```scheme
(define (accumulate combiner null-value term a next b)
  (define (iter a res)
    (if (> a b) res (iter (next a) (combiner (term a) res))))
  (iter a null-value))
```

## 4.9.2 Product en Sum
```scheme
(define (product factor a next b)
  (accumulate * 1 factor a next b))

(define (sum term a next b)
  (accumulate + 0 term a next b))

(define (accumulate combiner null-value term a next b)
  (define (iter a res)
    (if (> a b) res (iter (next a) (combiner (term a) res))))
  (iter a null-value))
```

## 4.9.3 Add en Multiply
```scheme
(define (add a b)
  (accumulate + a (lambda (x) 1) 1 (lambda (x) (+ x 1)) b))


(define (multiply a b)
  (accumulate + 0 (lambda (x) a) 1 (lambda (x) (+ 1 x)) b))

(define (accumulate combiner null-value term a next b)
  (define (iter a res)
    (if (> a b) res (iter (next a) (combiner (term a) res))))
  (iter a null-value))
```

## 4.10.1 Filtered-accumulate
```scheme
(define (filtered-accumulate combiner filter? null-value term a next b)
  (define (iter a res)
    (cond ((> a b) res)
          ((filter? a) (iter (next a) (combiner (term a) res)))
          (else (iter (next a) res))))
    (iter a null-value))
```

## 4.10.2 Grootste gemene deler
```scheme
(define (product-gcd n)
  (define (iter i res)
    (cond ((= n i) res)
          ((= (gcd i n) 1) (iter (+ 1 i) (* res i)))
          (else (iter (+ 1 i) res))))
  (iter 1  1))
```


