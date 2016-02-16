# Pre and Post assertion fucntions

Clojure comes with a few special forms that help aid writing clojure code.`:pre` and `:post` helps assert if pre and post conditions to a function and the function is only executed successfuly if passed argument/the result matches these assertions

- `:pre` - helps put condition on the input argument
- `:post` - helps put condition on the output of the function

```clojure
(defn 4sqr
  [x]
  {:pre [(pos? x)]
   :post [(= % 16)]}
  (* x x))
```
This function will only run successfuly when `x = +4`, for any other input the function will return an `AssertionError`