# Lazyness in clojure

Today I was wondering if the lazy sequence in clojure was really an advantage over the non-lazy(_eager_/_realised_) sequence. Say I want to get the first n positive numbers. So I wrote a two versions of it, one lazy and the other eager.

```clojure
(defn lazy-first-n
	([] (lazy-first-n 1))
	([n] (lazy-seq (cons n (lazy-first-n (inc n))))))
```
```clojure
(defn eager-first-n
  [n]
  (loop [k n
         final '()]
    (if (= 0 k)
      final
      (recur (dec k) (conj final k)))))
```

Now if I want to write a program that prints first n numbers, I could have used the any of these two implementation and it would not have made much difference. But consider the following case

```clojure
(defn foo
  [x]
  (println "I've got x !")
  true)
```

Now lets run this while passing a list of first 10 million positive integers (_humour me_).
```clojure
user=> (time (foo (take 10000000 (lazy-first-n))))
I've got x !
"Elapsed time: 0.299689 msecs"
true

user=> (time (foo (first-n 10000000)))
I've got x !
"Elapsed time: 5807.555375 msecs"
true
```

Passing a lazy list, isn't evaluated until it is used. This is gives a big advantage when you are using big sequences. There might be cases where we need to use them and cases where we might not need, so by delaying the realisation of the list we make sure that it is only realised/evaluated if and only if we need it.
