# Day 3

- Create a new data type by using `data` keyword

```haskell
data User = NormalUser { usrName :: String, usrAge :: Int }
     	  | Admin { usrName :: String, pass :: String}
	  deriving (Show)
```

- While using in the repl make sure to add, `deriving (Show)`
- create a binding using the constructor `NormalUser` / `Admin`
- `User` would be the name of the type
- access the value using the functions like `usrName`, `usrAge` & `pass`
- while not using the function values, you can use pattern matching to get the values
```haskell
let x (User name _) = print name
```