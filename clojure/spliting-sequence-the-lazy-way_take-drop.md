# Spliting sequences the lazy way in clojure

Tackling this problem in [4clojure](https://www.4clojure.com/problem/49), which requires to split into two parts without using the `split-at` function, you will appreciate both `take` and `drop` functions in clojure.

The primary purpose of these functions are to `take`/`drop` n elements of any sequence.

To split a sequence at an index could be achieved by this

```clojure
> (fn split-at-i
    [i a-list]
    (conj [] (take i a-list) (drop i a-list)))
> (split-at-i 3 [1 2 3 4 5 6 7])
; [(1 2 3) (4 5 6 7)]
```