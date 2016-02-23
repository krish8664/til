# Day 2

- `:i` to display info in the repl
- `:m +` add module to repl
- `:m -` remove module from repl

## List

- `++` - concat lists
- `length` - length of the list
- `cycle` - creates a lazy seq of a list

## String

- `"` for string
- `'` for character


## Functions

### Multiclause functions
```haskell
sumTill 1 = 1
sumTill n = n + sumTill (n - 1)
```

### Guard Functions
```haskell
sumTill' n
 | n == 1 = 1
 | otherwise = n + sumTill' (n - 1)
```
### Case exression
```haskell
sumTill'' x = case of
 1 -> 1
 _ -> x + sumTill'' (x - 1)
```

## List comprehension

```haskell
  [(x,y) | x <- [1 .. 10] , y <- [1 .. 10] , x == y]
```