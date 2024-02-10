## Lijsten
```scheme
(define (zip ls1 ls2 op)
  (if (null? ls1)
      '()
      (cons (op (car ls1) (car ls2))
            (zip (cdr ls1) (cdr ls2) op))))
(zip '(1 2 3 4) '(5 6 7 8) +)

(define (sum-of-squares ls1 ls2)
  (zip ls1 ls2 (lambda (x y) (+ (* x x) (* y y)))))
(sum-of-squares '(1 2 3 4) '(5 6 7 8))
```
## Destructieve operaties
```scheme
(define (kap-af ls m)
  (cond ((null? ls) 'ok)
        ((> (car ls) m)
         (set-car! ls m)
         (kap-af (cdr ls) m))
        ((null? (cdr ls)) 'ok)
        ((> 0 (cadr ls))
         (set-cdr! ls (cddr ls))
         (kap-af ls m))
        (else (kap-af (cdr ls) m))))

(define ls  '(1 2 -1 -2 8 9 4 2 -7))
(kap-af ls 3)
ls
```

## Bomen
```scheme
(define (verdeel-budget tree amount)
  (if (null? (children tree))
      (list (list (parent tree) amount ))
      (verdeel-in (children tree) (/ amount (length (children tree))))))

(define (verdeel-in tree amount)
  (if (null? tree)
      '()
      (append (verdeel-budget (parent tree) amount)
              (verdeel-in (children tree) amount))))
```
## Streams
```scheme
;voor te testen
(define (split stream n)
  (define (splitter stream i)
    (cond ((or (empty-stream? stream) (= i n))
           (cons the-empty-stream stream))
          (else (let ((tmp (splitter (tail stream) (+ i 1))))
                  (cons (cons-stream (head stream) (car tmp))
                        (cdr tmp))))))
 
  (if (empty-stream? stream)
      the-empty-stream
      (let ((tmp (splitter stream 0)))
        (cons-stream (car tmp) (split (cdr tmp) n)))))


(define (populairste s)
  (define (sum-heads s)
    (accumulate + 0 (map-stream (lambda (x) (head x)) s)))
  
  (define (loop s)
    (if (empty-stream? (head s))
        '()
        (cons (sum-heads s) (loop (map-stream (lambda (x) (tail x)) s)))))
  
  (define (get-index l i m mi)
    (cond ((null? l) mi)
          ((> (car l) m)
           (get-index (cdr l) ( + i 1) (car l) (+ i 1)))
          (else (get-index (cdr l) (+ i 1) m mi))))
  
  (let ((ls (loop s)))
    (get-index (cdr ls)0 (car ls) 0 )))

(define l   (split(list->stream '(1 2 3 4 5 6 7 8 9)) 3))
(define ll  (split(list->stream '(0 1 2 3 0 1 2 3 0 1 2 3 0 1 2 3)) 4))
(define lll (split(list->stream '(9 8 7 6 5 4 3 2 1)) 3))
(populairste l)
(populairste ll)
(populairste lll)
```