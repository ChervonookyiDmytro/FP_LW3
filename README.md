<p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПМ СПіСКС</b></p>
<p align="center">
<b>Звіт до лабораторної роботи 3</b><br/>
"Конструктивний і деструктивний підходи до роботи зі списками""<br/>
дисципліни "Вступ до функціонального програмування"
</p>

<p align="right"> 
<b>Студент</b>: 
Червоноокий Дмитро</p>

<p align="right"><b>Рік</b>: 2025</p>

## Загальне завдання
Реалізуйте алгоритм сортування чисел у списку двома способами: функціонально і
імперативно.

1. Функціональний варіант реалізації має базуватись на використанні рекурсії і
конструюванні нових списків щоразу, коли необхідно виконати зміну вхідного
списку. Не допускається використання: псевдо-функцій, деструктивних операцій,
циклів . Також реалізована функція не має бути функціоналом (тобто приймати на
вхід функції в якості аргументів).
2. Імперативний варіант реалізації має базуватись на використанні циклів і
деструктивних функцій (псевдофункцій). Не допускається використання функцій
вищого порядку або функцій для роботи зі списками/послідовностями, що
використовуються як функції вищого порядку. Тим не менш, оригінальний список
цей варіант реалізації також не має змінювати, тому перед виконанням
деструктивних змін варто застосувати функцію copy-list (в разі необхідності).
Також реалізована функція не має бути функціоналом (тобто приймати на вхід
функції в якості аргументів).

Кожна реалізована функція має бути протестована для різних тестових наборів. Тести
мають бути оформленні у вигляді модульних тестів (наприклад, як наведено у п. 2.3).

## Варіант 3

Алгоритм сортування обміном №2 (із використанням прапорця) за незменшенням.


## Лістинг функції з використанням конструктивного підходу
```lisp
(defun sort-lst-helper (tail r &optional (result nil) (is_swapped nil))
             (if (or (zerop r) (endp tail) (endp (cdr tail)))
               (values (append (reverse result) tail) is_swapped)
                (let ((a (car tail)) (b (cadr tail)))
                  (if (> a b)
                      (sort-lst-helper (cons a (cddr tail)) (1- r) (cons b result) t)
                      (sort-lst-helper (cdr tail) (1- r) (cons a result) is_swapped)))))

 (defun sort-lst (original-list)
  (labels ((my-sort (lst r)
             (if (<= r 0)
                 lst
                 (multiple-value-bind (result swapped)
                     (sort-lst-helper lst r)
                   (if swapped
                       (my-sort result (1- r))
                       result)))))
    (my-sort original-list (max 0 (1- (length original-list))))))

```
### Тестові набори та утиліти
```lisp
(defun check-first-algorithm (name input expected)
  "Execute `exchange4-constructive' on `input', compare result with `expected' and print
  comparison status."
  (format t "~:[FAILED~;PASSED~]... ~a~%"
          (equal (sort-lst input) expected)
          name))

 (defun test-first ()
  (check-first-algorithm "test 1.0" '(0) '(0))
  (check-first-algorithm "test 1.1" '(0 0) '(0 0))
  (check-first-algorithm "test 1.2" '(0 1) '(0 1))
  (check-first-algorithm "test 1.3" '(1 0) '(0 1))
  (check-first-algorithm "test 1.4" '(0 1 0 1) '(0 0 1 1))
  (check-first-algorithm "test 1.5" '(5 11 3 7 1 4 2 9) '(1 2 3 4 5 7 9 11))
  (check-first-algorithm "test 1.6" '(1 0.1) '(0.1 1))
  (check-first-algorithm "test 1.7" '(0 -1 -16 4 ) '(-16 -1 0 4)))
```
### Тестування
```lisp
CL-USER> (test-first)
PASSED... test 1.0
PASSED... test 1.1
PASSED... test 1.2
PASSED... test 1.3
PASSED... test 1.4
PASSED... test 1.5
PASSED... test 1.6
PASSED... test 1.7
NIL
```
## Лістинг функції з використанням деструктивного підходу
```lisp
(defun second-algorithm (original_lst &aux (flag t) (R (1- (length original_lst))) (lst (copy-list original_lst)))
           (loop while flag do
                 (setf flag nil)
                 (dotimes (i R)
                   (when (> (nth i lst) (nth (1+ i) lst))
                     (rotatef (nth i lst) (nth (1+ i) lst)) 
                     (setf flag t)))
                 (decf R))
           lst)
```
### Тестові набори та утиліти
```lisp
(defun check-second-algorithm (name input expected)
  "Execute `exchange4-constructive' on `input', compare result with `expected' and print
  comparison status."
  (format t "~:[FAILED~;PASSED~]... ~a~%"
          (equal (second-algorithm input) expected)
          name))

 (defun test-second ()
  (check-second-algorithm "test 2.0" '(0) '(0))
  (check-second-algorithm "test 2.1" '(0 0) '(0 0))
  (check-second-algorithm "test 2.2" '(0 1) '(0 1))
  (check-second-algorithm "test 2.3" '(1 0) '(0 1))
  (check-second-algorithm "test 2.4" '(0 1 0 1) '(0 0 1 1))
  (check-second-algorithm "test 2.5" '(5 11 3 7 1 4 2 9) '(1 2 3 4 5 7 9 11))
  (check-second-algorithm "test 2.6" '(1 0.1) '(0.1 1))
  (check-second-algorithm "test 2.7" '(0 -1 -16 4 ) '(-16 -1 0 4)))
```
### Тестування
```lisp
CL-USER> (test-second)
PASSED... test 2.0
PASSED... test 2.1
PASSED... test 2.2
PASSED... test 2.3
PASSED... test 2.4
PASSED... test 2.5
PASSED... test 2.6
PASSED... test 2.7
NIL
```


