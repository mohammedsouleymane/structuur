5.6 Element toevoegen achteraan een lijst
```scheme
(define (add-to-end n ls)
  (if (null? ls) (cons n '())
       (cons (car ls) (add-to-end n (cdr ls)))
    )
  )
```

5.7 Append
```scheme
(define (my-append li ls)
  (define (iter l n) 
    (if (null? l) n
        (iter (cdr l) (cons (car l) n ))))
  (iter (reverse li) ls))
```

5.8 Lijsten reversen
```scheme
(define (rec-reverse l)
  (if (null? l) l
      (append (rec-reverse (cdr l)) (list (car l)))
      )

  )

(define (iter-reverse l)
  (define (iter l res) 
    (if (null? l) res
        (iter (cdr l) (cons (car l) res )))
    )
  (iter l '()))
```

5.9 Laatste element van lijst
```scheme
(define (last l)
  (cond ((null? l) #f)
        ((null? (cdr l)) (car l))
        (else (last (cdr l))))
  )
```

5.10.1 Change
```scheme
(define (change e1 e2 l)
  (cond ((null? l) '())
        ((eq? e1 (car l)) (cons e2 (change e1 e2 (cdr l))))
        (else (cons (car l) (change e1 e2 (cdr l))))
    )
  )
```

5.10.2 Change d.m.v. map
```scheme

(define (change-dmv-map e1 e2 l)
  (map (lambda (x) (if (eq? x e1) e2 x)) l))
```

 5.11 My-equal?
 ```scheme
 (define (my-equal? l1 l2)

  (cond ((eq?  l1 l2))
        ((or (null? l1) (null? l2)) #f)
        ((eq? (car l1) (car l2) ) (my-equal? (cdr l1) (cdr l2)))
        (else #f)))
 ```

 5.12.1 Sommatie van elementen in lijsten: Recursieve versie
 ```scheme
 (define (rec-sum-lists l1 l2)
  (cond ((and (null? l1) (null? l2) '()))
        ((null? l1) (cons (car l2) (rec-sum-lists '() (cdr l2))))
        ((null? l2) (cons (car l1) (rec-sum-lists '() (cdr l1))))
        (else (cons (+ (car l1) (car l2)) (rec-sum-lists (cdr l1) (cdr l2))))))
 ```

5.12.2 Sommatie van elementen in lijsten: Iteratieve versie
 ```scheme
 (define (iter-sum-lists l1 l2)
  (define (iter l1 l2 res)
    (cond ((and (null? l1) (null? l2) res))
           ((null? l1) (iter l1 (cdr l2) (cons (car l2) res)))
           ((null? l2) (iter (cdr l1) l2 (cons (car l1) res)))
           (else (iter (cdr l1) (cdr l2) (cons (+ (car l1) (car l2)) res)))))
  (reverse (iter l1 l2 '())))
 ```

 5.15.1 Samenvoegen van twee lijsten
 ```scheme
 (define (rec-merge-n l1 l2 n)
  (define (rec l1 l2 n2)
    (cond ((and (null? l1) (null? l2) ) '())
          ((null? l1) (rec l2 l1 0))
          ((= n2 n)  (rec l2 l1 0))
          (else (cons (car l1) (rec (cdr l1) l2 (+ 1 n2))))
          )
    )
  ;(trace rec)
  (rec l1 l2 0)
  )


(define (iter-merge-n l1 l2 n)

  (define (iter l1 l2 n2 res)
    (cond ((and (null? l1) (null? l2) ) (reverse res))
          ((null? l1) (iter l2 l1 0 res))
          ((= n2 n) (iter l2 l1 0 res))
          (else (iter (cdr l1) l2 (+ n2 1) (cons (car l1) res)))
          )
    )
  ; (trace iter)
  (iter l1 l2 0 '())
  )
 ```

5.15.2 Lijsten samenvoegen: Willekeurig aantal lijsten
```scheme
(define (super-merge-n l1 n)

  (define (iter l1 s n2 res)
    
    (cond ((and (null? l1) (null? s) ) (reverse res))
          ((null? l1) (iter (car s) (cdr s) 0 res))
          ((= n2 n) (iter (car s) (append (cdr s) (list l1)) 0 res))
          (else (iter (cdr l1)  s (+ n2 1) (cons (car l1) res)))
          )
    )
 ;  (trace iter)
  (iter (car l1) (cdr l1) 0 '())
  )
```