## 7.2.1 Aantal element van een boom
```scheme
  (define (atom? x)
    (not (pair? x)))

 (define (leaf-count tree)
    (cond ((null? tree) 0)
          ((atom? tree) 1)
          (else (+ (leaf-count (car tree)) (leaf-count (cdr tree))))))
```

## 7.2.2 Diepte van een boom
```scheme
(define (atom? x)
    (not (pair? x)))

(define (depth tree)
  (cond ((or (null? tree) (atom? tree)) 0)
        (else
          (max (+ 1 (depth (car tree))) (depth (cdr tree))))))
```

## 7.2.3 Diepte en aantal elementen van een boom
```scheme
(define (atom? x)
  (not (pair? x)))

(define (depth-and-leaf-count tree)
  
  (define (values tree)
    (define left (depth-and-leaf-count (car tree)))
    (define right  (depth-and-leaf-count (cdr tree)))
    (cons (max (+ 1 (car left)) (car right)) (+ (cdr left) (cdr right))))
  
  (cond ((null? tree) (cons 0 0))
        ((atom? tree) (cons 0 1))
        (else (values tree))))
```

## 7.3 Fringe
```scheme
 (define (atom? x)
    (not (pair? x)))

 (define (fringe tree)
    (cond ((null? tree) '())
          ((atom? tree) (cons tree '()))
          (else (append (fringe (car tree)) (fringe (cdr tree))))))
```

## 7.5 Structuur vergelijken
```scheme
(define (atom? x)
  (not (pair? x)))
(define (same-structure? l1 l2)

  (cond ((null? l1) (null? l2))
        ((atom? l1) (atom? l2))
        (else (and (same-structure? (car l1) (car l2)) (same-structure? (cdr l1) (cdr l2))))))
```

## 7.6.1 Deep-combine
```scheme
(define (atom? x)
  (not (pair? x)))
(define (deep-combine combiner null-value l)

  (cond ((null? l) null-value)
        ((atom? l) l)
        (else (combiner (deep-combine combiner null-value (car l)) (deep-combine combiner null-value (cdr l))))))
```

## 7.6.2 Deep-map
```scheme
(define (atom? x)
  (not (pair? x)))

(define (deep-map f l)

  (cond ((null? l) '())
        ((atom? l) (f l))
        (else (cons (deep-map f (car l)) (deep-map f (cdr l))))))
```

## 7.6.3 Deep-change
```scheme
(define (atom? x)
  (not (pair? x)))

(define (deep-change e1 e2 l)
  (cond ((null? l) '())
        ((atom? l) (if (eq? l e1) e2 l))
        (else (cons (deep-change e1 e2 (car l)) (deep-change e1 e2 (cdr l))))))


;with deep map
(define (deep-map f l)
  (cond ((null? l) '())
        ((atom? l) (f l))
        (else (cons (deep-map f (car l)) (deep-map f (cdr l))))))
(define (deep-change e1 e2 l)
  (deep-map (lambda (x) (if (eq? x e1) e2 x)) l))
```

## 7.6.4 Deep-atom-member
```scheme
(define (atom? x)
  (not (pair? x)))

(define (deep-atom-member? e l)
  (cond ((null? l) #f)
        ((atom? l) (eq? l e))
        (else (or (deep-change e1 e2 (car l)) (deep-change e1 e2 (cdr l))))))


; with deep-combine and map
(define (atom? x)
  (not (pair? x)))

(define (deep-map f l)
  (cond ((null? l) '())
        ((atom? l) (f l))
        (else (cons (deep-map f (car l)) (deep-map f (cdr l))))))

(define (deep-combine combiner null-value l)

  (cond ((null? l) null-value)
        ((atom? l) l)
        (else (combiner (deep-combine combiner null-value (car l)) (deep-combine combiner null-value (cdr l))))))


(define (deep-atom-member? e l)
  (> (deep-combine + 0 (deep-map (lambda (x) (if (eq? x e) 1 0)) l)) 0))
```

## 7.6.5 Count atoms
```scheme
(define (atom? x)
  (not (pair? x)))

(define (deep-map f l)
  (cond ((null? l) '())
        ((atom? l) (f l))
        (else (cons (deep-map f (car l)) (deep-map f (cdr l))))))

(define (deep-combine combiner null-value l)

  (cond ((null? l) null-value)
        ((atom? l) l)
        (else (combiner (deep-combine combiner null-value (car l)) (deep-combine combiner null-value (cdr l))))))

(define (count-atoms l)
  (deep-combine + 0 (deep-map (lambda (x) (if (atom? x) 1 0)) l))) 
```

## 7.9.1 Tel bladeren
```scheme
(define (atom? x)
  (not (pair? x)))

(define (leafs l)

  (cond ((null? l) 0)
        ((atom? l) (if (eq? l 'blad) 1 0))
        (else (+ (leafs (car l)) (leafs (cdr l))))))
```

## 7.9.2 Zoek alle appels
```scheme
(define (atom? x)
  (not (pair? x)))

(define (all-apples l)
  (cond ((or (atom? l)(null? l)) '())
        ((eq? (car l) 'appel) (cons (cdr l) '()))
        (else (append (all-apples (car l))  (all-apples (cdr l))))))
```

## 7.9.3 Geef de verschillende soorten appels
```scheme
(define (atom? x)
  (not (pair? x)))

(define (all-apples l)
  (cond ((or (atom? l)(null? l)) '())
        ((eq? (car l) 'appel) (cons (cdr l) '()))
        (else (append (all-apples (car l))  (all-apples (cdr l))))) )

 
(define (distinct l) ;takes flattend list & removes duplicates
  (define ll (if(not (null? l)) (distinct (cdr l))))
  (cond ((null? l) '())
        ((not (contains (car l) ll)) (cons (car l) ll))
        (else ll)))

(define (contains e l) ;takes flattend list & checks if e is in list
  (cond ((null? l) #f)
        ((equal? (car l) e))
        (else (contains e (cdr l)))))

(define (apple-types l)
  (distinct (all-apples l)))
```

## 7.9.4 Procedure om de boom te bewerken
```scheme
(define (atom? x)
  (not (pair? x)))

(define (bewerk-boom boom doe-blad doe-appel combiner init)
  (cond ((null? boom) init)
        ((atom? boom) (if (eq? boom 'blad) (doe-blad boom)))
        ((eq? (car boom) 'appel) (doe-appel boom))
        (else (combiner
               (bewerk-boom (car boom) doe-blad doe-appel combiner init)
               (bewerk-boom (cdr boom) doe-blad doe-appel combiner init)))))
```

## 7.9.5 Tel bladeren (hogere orde)
```scheme
(define (atom? x)
  (not (pair? x)))

(define (bewerk-boom boom doe-blad doe-appel combiner init)
  (cond ((null? boom) init)
        ((atom? boom) (if (eq? boom 'blad) (doe-blad boom)))
        ((eq? (car boom) 'appel) (doe-appel boom))
        (else (combiner
               (bewerk-boom (car boom) doe-blad doe-appel combiner init)
               (bewerk-boom (cdr boom) doe-blad doe-appel combiner init)))))


(define (leafs-dmv-bewerk boom)
  (bewerk-boom boom (lambda (x) 1) (lambda (x) 0) + 0))
```

## 7.9.6 Geef alle appels (hogere orde)
```scheme
(define (atom? x)
  (not (pair? x)))

(define (bewerk-boom boom doe-blad doe-appel combiner init)
  (cond ((null? boom) init)
        ((atom? boom) (if (eq? boom 'blad) (doe-blad boom)))
        ((eq? (car boom) 'appel) (doe-appel boom))
        (else (combiner
               (bewerk-boom (car boom) doe-blad doe-appel combiner init)
               (bewerk-boom (cdr boom) doe-blad doe-appel combiner init)))))

(define (all-apples-dmv-bewerk boom)
  (bewerk-boom boom (lambda (x) '()) (lambda (x) (cons (cdr x) '())) append '()))
```
