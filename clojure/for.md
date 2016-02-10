# List comprehension using for

For makes list comprehension in clojure easy. I was trying to find a way to merge two list into a third list which will have an elemet of the first list matched with every other element of the second list, you know like a cartesian product. Even though I was able to write it without using for, I realised the existence of such a construct then. For creates a lazy sequence.

```clojure
> (for [x '(1 2) y '(5 6)] [x y])
; ([1 5] [1 6] [2 5] [2 6])
```

Now `for` definitly does this, but it is not limited to this. Its way more powerful than this. `for` also supports modifers like `:let`, `:when` & `:while`. This means that `for` wil be able to do something like this

```clojure
> (for [x [0 1 2 3 4 5]
        :let [y (+ 1 x)]
        :when (even? x)]
   [x y])
; ([0 1] [2 3] [4 5])
```