## 9.3 Lussen
```scheme
(define (make-ring n)
  (define last '())
  (let loop ((i 0)
             (res '()))
    (if (= i 1) (set! last res)) ;set last as (0) element
    (if (<= i n)  (loop (+ 1 i) (cons i res))
        (begin (set-cdr! last res) ;sets last cdr to the list
               res))))
```

## 9.4.1 left-rotate
```scheme
(define (left-rotate r)
  (set! r (cdr r))
  r)
```

## 9.4.2 right-rotate
```scheme
(define (right-rotate r)
  (define n (cons 0  0 ))
  (define (loop f)
    (cond ((null? f) #f)
          ((eq? r (cdr  f))   f)
          (else (loop (cdr f))) ))
  (loop r))
```

## 9.5 Cycles detecteren
```scheme
(define (cycles? r)
  (define possible-starts '())
  (define (loop f)
    (cond ((null? f) #f)
          ((memq f possible-starts) #t)
          (else (set! possible-starts (cons f possible-starts)) (loop (cdr f)))))
  (loop r))
```

## 9.8 Correcte versie count-pairs
```scheme
(define (count-pairs p)
  (define members '())
  (let loop ((l p))
    (cond ((not(pair? l)) 0)
          ((memq l members) 0)
          (else (set! members (cons l members))
                (+ 1 (loop  (cdr l))
                   (loop  (car l)))))))
```

## 9.13 Examen Informatica 2de zit 1995
```scheme
(define (schuif-in! l1 l2)
  (if (null? (cdr l1)) (set-cdr! l1 l2)
      (begin
        (schuif-in!  l2 (cdr l1))
        (set-cdr! l1 l2))))

(define (schuif-in-alt! l1 l2)
  (if (null? (cdr l1))
      (set-cdr! l1 l2)
      (let ((next1 (cdr l1))
            (next2 (cdr l2)))
        (set-cdr! l1 l2)
        (set-cdr! l2 next1)
        (schuif-in-alt! next1 next2)))) 
```
## 9.17.2 Samenvoegen van klantenbestanden
```scheme
(define (symbol<? s1 s2)
  (string<? (symbol->string s1) (symbol->string s2)))

(define (element=? el1 el2)
  (equal? el1 el2))

(define best1 '((ann (meiboomstraat 12 1820 Eppegem))
                (bert (populierendreef 7 1050 Brussel))
                (kurt (Mechelsesteenweg 50 1800 Vilvoorde))))

(define best2 '((bert (populierendreef 7 1050 Brussel))
                (jan (eikestraat 1 9000 Gent))
                (sofie (boerendreef 5  2800 Mechelen))
                (q ())))

(define (merge b1 b2)
  (define (insert l el)
  (cond ((null? l) l)
        ((element=? (car el) (car l)) l)
        ((symbol<? (caar el)  (caar l)) 
                   (set-cdr! el  l) el)
        ((null? (cdr l)) (set-cdr! l el) l)
        (else (set-cdr! l (insert (cdr l) el)) l)))
  
  (for-each (lambda (x) (insert b1 (list x))) b2)
  b1)

(define (merge-2 b1 b2)
  (define (hulp c1 c2 prev)
    (cond ((null? c1) (set-cdr! prev c2))
          ((null? c2) (set-cdr! prev c1))
          ((best-eq? c1 c2)
           (set-cdr! prev c1)
           (hulp (cdr c1) (cdr c2) c1))
          ((best-< c1 c2)
           (set-cdr! prev c1)
           (hulp (cdr c1) c2 c1))
          (else (set-cdr! prev c2)
                (hulp c1 (cdr c2) c2))))
    (let ((dummy (cons 'dummy '())))
          (hulp b1 b2 dummy)
          (cdr dummy)) )
```

## 9.14 Examen informatica 1ste zit 1996
```scheme
(define (ontdubbel! li)
  (cons (let loop ((even '())
                   (l li))
          (cond ((null? l)  even)
                ((null? (cdr l) )  even)
                ((even? (cadr l))
                 (set! even  (append even (list (cadr l))))
                 (set-cdr! l (cddr l))
                 (loop even  l))
                (else (loop even (cdr l))))) li))
```

## 9.15 Examen januari 2004: Destructief
```scheme
(define (insert! lst1 lst2)
  (define (loop-mini lst e)
    (if (null? (cdr lst))
        (begin
          (set-cdr! e '())
          (set-cdr! lst e))
        (loop-mini (cdr lst) e) ))

  (let loop ( (l lst1)
              (l2 lst2))
    (if (not (null? l))
        (begin (loop (cdr l) (cdr l2))
               (loop-mini (car l)  l2)))) lst1)
```
