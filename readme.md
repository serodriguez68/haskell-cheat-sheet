WORK IN PROGRESS.

# Characteristics of Haskell
+ Strong Type
+ Compiled (e.g of compiler ghc)
+ Extension of files .hs
+ ghci -> Interactive environment for Haskell
+ It is possible to run a Haskell program as a standalone executable if the special `main` function is defined.
+ There is no native concept of sequence, branching or loops.
+ A Haskell program is one big expression to be evaluated with determined inputs.
    + Small-scale functions get combined to form larger-scale functions. Larger-scale functions are combined in the same way to form even larger functions, until eventually we have a complete program.
    + A simple way to combine functions is to take the result of one function application and make that the input argument of another. 

# Functions in Haskell
+ Functions are __pure__: no side effects, have no state
+ Functions are defined using __expressions__ (as opposed to sequence of steps like OO programming languages).
+ Haskell doesn't use parentheses to separate functions from their arguments. In both function definition and use, we use spaces instead: it's `f 3` rather than `f(3)`

```haskell
-- | 1. Define function f whose input is Int and output is Int
--   2. f gets an argument x and the definition of the function is x ^2 
f :: Int -> Int
f x = x^2

-- | 1. Define function g which has 2 inputs of type Int and Int and output of type Int
--   2. g gets arguments x and y and the definition of the funtion is x * y 
g :: Int -> Int -> Int
g x y = x * y
```
