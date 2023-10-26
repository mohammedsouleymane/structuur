## 7.11.1 Bazen
```scheme
(define parent  car)
(define children  cdr)

(define (add in-list tree)
  (if in-list (append in-list (list (parent tree))) in-list))

(define (bazen-van tree el)
  (cond ((eq? (parent tree) el) '())
        (else(add (bazen-van-in (children tree) el) tree))))

(define (bazen-van-in tree el)
  (cond ((null? tree) #f)
        (else (or (bazen-van (car tree) el)
                  (bazen-van-in (cdr tree) el)))))
```
## 7.11.2 Hiërarchisch
```scheme
(define parent  car)
(define children  cdr)
  
(define (hierarchisch? p1 p2 organigram)
  (cond ((eq? (parent organigram) p1) (if (eq? p2 #t) p2 (hierarchisch-in #t p2 (children organigram))))
        ((eq? (parent organigram) p2)  (if(eq? p1 #t) p1 (hierarchisch-in p1 #t (children organigram))))
        (else (hierarchisch-in p1 p2 (children organigram)))))

(define (hierarchisch-in p1 p2 organigram)
  (cond ((null? organigram) #f)
        (else (or (hierarchisch? p1 p2 (car organigram))
                  (hierarchisch-in p1 p2 (cdr organigram))))))
```

## 7.11.3 Collega's
```scheme
;keeping path
(define (baas boom)
  (car boom))

(define (onderlingen boom)
  (cdr boom))
(define (atom? x)
  (not (pair? x)))

(define (fringe tree)
  (cond ((null? tree) '())
        ((atom? tree) (cons tree '()))
        (else (append (fringe (car tree)) (fringe (cdr tree))))))

(define (collegas organigram p)
  (define (boom org pad)
    (if (eq? (baas org) p)
        (append pad (fringe (onderlingen org)))
        (bomen (onderlingen org) (cons (baas org) pad))))
  
  (define (bomen orgs pad)
    (if (null? orgs)
        #f
        (or
        (boom (car orgs) pad)
        (bomen (cdr orgs) pad))))
  
  (boom organigram '()))

;not final
(define parent  car)
(define children  cdr)

(define (add in-list tree)
  (if in-list (append in-list (list (parent tree))) in-list))

(define (atom? x)
  (not (pair? x)))

(define (fringe tree)
  (cond ((null? tree) '())
        ((atom? tree) (cons tree '()))
        (else (append (fringe (car tree)) (fringe (cdr tree))))))

(define (collegas el tree)

  (define (ch el tree r a)
    (cond ((eq? (parent tree) el) (r tree))
          (else (a (ch-in (children tree) el r a) tree))))

  (define (ch-in tree el r a)
    (cond ((null? tree) #f)
          (else (or (ch el (car tree) r a)
                    (ch-in (cdr tree) el r a)))))

  (append (ch el tree (lambda (x) '()) add) (ch el tree (lambda (x) (fringe (cdr x))) (lambda (x y) x))))
```

## 7.13.1 print-vanaf
```scheme
(define (display-n n d)
  (cond ((> n 0) (display d)
                 (display-n (- n 1) d))))
 
(define (println aantalblanco tekst)
  (display-n aantalblanco " ")
  (display tekst)
  (newline))

(define parent  car)
(define children  cdr)

(define (print-vanaf  tree el)
  (cond ((number? el) (println el (parent tree)) (print-vanaf-in (children tree) (+ 1 el)) tree)
        ((eq? (parent tree) el) (print-vanaf tree 0))
        (else (print-vanaf-in (children tree) el))))

(define (print-vanaf-in  tree el)
  (cond ((null? tree) (display ""))
        (else (begin (print-vanaf  (car tree) el) (print-vanaf-in (cdr tree) el)))))
```

## 7.13.2 print-tot
```scheme
(define (display-n n d)
  (cond ((> n 0) (display d)
                 (display-n (- n 1) d))))
 
(define (println aantalblanco tekst)
  (display-n aantalblanco " ")
  (display tekst)
  (newline))

(define parent  car)
(define children  cdr)


(define (print-tot tree n)
  (define (iter tree i)
    (if (<= i n)
        (begin (println i (parent tree))
               (print-tot-in (children tree) i))))
  
  (define (print-tot-in  tree n)
    (cond ((null? tree) (display ""))
          (else (begin (iter (car tree) (+ 1 n)) (print-tot-in (cdr tree) n)))))
  
  (iter tree 0))
```

## 17.7.1 Verdeel democratisch
```scheme
 (define (atom? x)
    (not (pair? x)))

(define (verdeel-democratisch tree amount)
  (define (leaf-count tree)
    (cond ((null? tree) 0)
          ((atom? tree) 1)
          (else (+ (leaf-count (car tree)) (leaf-count (cdr tree))))))
  (/ amount ( - (leaf-count tree) 1)))
```

## 7.17.2 Bereken budget
```scheme
(define parent  car)
(define children  cdr)


;(define (budget  tree a n)
;  (cond ((null? tree) 0)
;        ((= n 1) (begin (+ (car a) (budget-in (children tree) a n))))
;        ((= n 2) (begin (+ (cadr a) (budget-in (children tree) a n))))
;        ((= n 3) (begin (+ (caddr a) (budget-in (children tree) a n))))
;        (else (budget-in (children tree) a n))))

(define (budget tree a)
  (define (bud  tree a n)
    (cond ((null? tree) 0)
          ((and (> n 0) (>= (length a) n)) (+ (list-ref a (- n 1)) (budget-in (children tree) a n)))
          (else (budget-in (children tree) a n))))

  (define (budget-in  tree a n)
    (cond ((null? tree)0)
          (else (+ (bud (parent tree) a (+ 1 n)) (budget-in (children tree) a n)))))
  (bud tree a 0))


(define familieboom '(jan (piet (frans (tom)
                                       (roel))
                                (mie))
                          (bram (inge (bert (ina)
                                            (ilse))
                                      (bart))
                                (iris))
                          (joost (els (ilse)))))
```

## 7.17.3 Verdeel budget onder nakomelingen zonder kinderen
```scheme
(define parent  car)
(define children  cdr)

(define (verdeel tree n)
  
  (cond ((null? tree) '())
        ((null? (children tree)) (list (list (parent tree) n )))
        (else (verdeel-in (children tree) n))))

(define (verdeel-in tree n)
  (define amount (if (null? tree) n (/ n (length tree))))
  (if   (null? tree) '()
        (append (verdeel (car tree) amount) (verdeel-in (children tree) (- n amount)))))
(define familieboom '(jan (piet (frans (tom)
                                       (roel))
                                (mie))
                          (bram (inge (bert (ina)
                                            (ilse))
                                      (bart))
                                (iris))
                          (joost (els (ilse)))))
```
