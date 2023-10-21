## Opbrengst
```scheme
;iterative
(define (opbrengst start interest jaar)
  (if (= jaar 0)
      start
      (opbrengst (* start (/ (+ 100 interest) 100.0)) interest (- jaar 1)))) 
;start keep updating, if interest = 3 -> start * 1.03 

;recursive
(define (opbrengst start interest jaar)
  (if (= jaar 0)
      start
      (* (/ (+ 100 interest) 100.0) (rec start interest (- jaar 1)))))
```
