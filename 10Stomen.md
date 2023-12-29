##  10.1 reeksontwikkelingen
```scheme
(load "streams.rkt")

(define (fac n)
  (if (= n 0) 1 
      (* n (fac (- n 1)))))
(define (change x)
 (if (= (modulo (+ 1 x) 4) 0) -1 1))

(define (calc-e n)
  (accumulate + 0 (map-stream (lambda (x) (/ 1 (fac x)))
                              (enumerate-interval 0 n))))

(define (sinus e n)
  (accumulate + 0
              (map-stream
               (lambda (x) (* (change x ) (/ (expt e x) (fac x))))
               (streamfilter odd? (enumerate-interval 0 (- (* 2 n) 1))))))
```

## 10.2 som-kwadraten-oneven-elementen
```scheme
(load "streams.rkt")

(define (sum-odd-squares l)
  (accumulate + 0
              (map-stream
               (lambda (x) (expt x 2))
               (streamfilter odd? l))))
```

## 10.3 triples
```scheme
(load "streams.rkt")

(define (odd-sum-triples max)
  (let ((odds (streamfilter odd? (enumerate-interval 1 (- max 1)))))
    (map-stream (lambda (x) (list (car x) (cdr x) (+ (car x) (cdr x)))) (pairs odds odds))))
```

## 10.7 integers
```scheme
(define integers (cons-stream 1 (map-stream (lambda (x) (+ 1 x)) integers)))
```

## 10.8 streamfilter
```scheme
(load "streams.rkt")

(define integers (cons-stream 1 (map-stream (lambda (x) (+ 1 x)) integers)))

(define (not-mod x y)
  (not (= (modulo x y) 0)))

(define (integers-special s)
  (streamfilter (lambda (x) (and (not-mod x 2) (not-mod x 3) (not-mod x 5))) s ))
```

## 10.9 triplets
```scheme
(load "streams.rkt")

(define integers (cons-stream 1 (map-stream (lambda (x) (+ 1 x)) integers)))

(define (triplets)
 (streamfilter (lambda (x) (> (+ (car x) (cadr x)) (caddr x))) (cartesian-product integers integers integers)))
```

## 10.12.1 cut
```scheme
(define (half s)
  (cons (streamfilter (lambda (x) (= (head s) x)) s)
        (streamfilter (lambda (x) (not(= (head s) x))) s)))
  
  
(define (cut s)
  (if (not (empty-stream? s))
      (let ((half (half s))) 
             (cons-stream (car half) (cut (cdr half)))
             ) s)) 
```

## 10.12.2 merge
```scheme
(define (merge s s2)
  (cond ((empty-stream? s) s2)
        ((empty-stream? s2) s)
        ((< (head s) (head s2)) (cons-stream (head s) (merge (tail s) s2)) )
        (else (cons-stream (head s2) (merge s (tail s2))))))
```

## 10.12.3 merge-n
```scheme
(load "streams.rkt")

(define (merge s s2)
  (cond ((empty-stream? s) s2)
        ((empty-stream? s2) s)
        ((< (head s) (head s2)) (cons-stream (head s) (merge (tail s) s2)) )
        (else (cons-stream (head s2) (merge s (tail s2))))))

 (define (merge-n s)
   (let loop((s1 (head s))
             (s2 (tail s)))
     (cond ((empty-stream? s1) s2)
        ((empty-stream? s2) s1)
        (else (loop (merge s1 (head s2)) (tail s2))))))
```

## 10.12.4 traffiek-in-een-pretpark
```scheme
(load "streams.rkt")

(define (half s)
  (cons (streamfilter (lambda (x) (= (head s) x)) s)
        (streamfilter (lambda (x) (not(= (head s) x))) s)))
  
  
(define (cut s)
  (if (not (empty-stream? s))
      (let ((half (half s))) 
             (cons-stream (car half) (cut (cdr half)))
             ) s))

(define (merge s s2)
  (cond ((empty-stream? s) s2)
        ((empty-stream? s2) s)
        ((< (head s) (head s2)) (cons-stream (head s) (merge (tail s) s2)) )
        (else (cons-stream (head s2) (merge s (tail s2))))))

 (define (merge-n s)
   (let loop((s1 (head s))
             (s2 (tail s)))
     (cond ((empty-stream? s1) s2)
        ((empty-stream? s2) s1)
        (else (loop (merge s1 (head s2)) (tail s2))))))

 (define (count s)
   (if (empty-stream? s) 0
       (+ 1 (count (tail s))))) ;change to accumulate
 
 (define (pretpark-traffiek s)
   (map-stream (lambda (x)
                 (cons (car x) (count x)))
               (cut (merge-n s))))
```

## 10.16.1 prune
```scheme
(load "streams.rkt")

(define (prune s n)

  (let loop ((i 1)
             (st s))
   (cond ((empty-stream?  st) the-empty-stream)
         ((= i (* 2 n)) (loop 1 (tail st)))
         ((<= i n) (cons-stream (head st) (loop (+ i 1) (tail st))))
         (else (loop (+ 1 i) (tail st))))))
```
