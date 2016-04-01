# Lazyness in clojure

I was playing arund with the `lazy-seq` that clojure has and began to wonder if the lazy sequence really had an advantage over the non-lazy(_eager_/_realized_) sequence. So I wrote a two functions to generate the first n positive integers, one lazy and the other eager.

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

lets run this while passing a list of first 10 million positive integers (_humour me_).
```clojure
user=> (time (foo (take 10000000 (lazy-first-n))))
I've got x !
"Elapsed time: 0.299689 msecs"
true

user=> (time (foo (eager-first-n 10000000)))
I've got x !
"Elapsed time: 5807.555375 msecs"
true
```
Here in the first case `take` returns a lazy sequence, but the sequence isn't realized, so the function `foo` runs without ever having to use the argument. Whereas in the second case the `eager-first-n` the function first gets evaluated i.e generate the whole list and only then then function foo is invoked even though we are not using the argument anywhere within the function. This is gives a big advantage when you are working with really large sequences. It is likely there might be cases where we need to use them and cases where we might not need, so by delaying the realization of the list we make sure that it is only realized/evaluated if and only if and when it is needed.