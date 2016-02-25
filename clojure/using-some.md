# Using some

Some lets you check if anything in a collection satisfies the predicate. It will return first logical true vale of the predicate else nil.

Usage
```clojure
(some even? '(1 2 3))
true
(some even? '(1 3 4))
nil
```

It is also used with a set as the perdicate, to check if any of the elements of the set are there in the collection. It returns the first element in the collection, else nil

```clojure
(some #{2 4} '(1 3 4 5 6))
4
```