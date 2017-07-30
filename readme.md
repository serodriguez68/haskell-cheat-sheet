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
-- 1. Define function f whose input is Int and output is Int
-- 2. f gets an argument x and the definition of the function is x ^2 
f :: Int -> Int
f x = x^2

-- 1. Define function g which has 2 inputs of type Int and Int and output of type Int
-- 2. g gets arguments x and y and the definition of the funtion is x * y 
g :: Int -> Int -> Int
g x y = x * y
```

# Recurssion
+ The way of doing iteration in Haskell.
+ __Always__ make sure that your recurssion eventually converges.
```haskell
fact :: Integer -> Integer
fact 0 = 1
fact n = n * fact (n-1)
-- Parenthesis around (n-1) are used because a function has higher precedence than a substraction. 
-- Without parenthesis this would be interpreted as `(f n) - 1
```

# Pattern matching
+ A function definition can have multiple pattern definitions (for example, in `fact 0` 0 is a pattern to match and in `fact n` n is a pattern to match)
+ Patterns are evaluated in top-down order and only the FIRST matched pattern is excecuted
    + If no patterns match => RUNTIME ERROR 
    + BEST PRACTICE: make your patterns mutually exclusive + collectively exhaustive.
    + If no pattern is matched => Infinite loop.
    ```haskell
    fact :: Integer -> Integer
    -- Pattern 1
    fact 0 = 1
    -- Pattern 2
    fact n = n * fact (n-1)
    ```

# Guards
+ Definition of functions with multiple cases using booleans.
+ Guards are evaluated in top-down order and only the FIRST matched guard is excecuted.
+ `otherwise` will always evaluate to true.
``` haskell
fact :: Integer -> Integer
fact n
    | n < 0     = 0
    | n == 0    = 1
    | otherwise = n * fact (n-1)
```

# Types
+ __Integer:__infinite precision integer type (Store any number, however large (well, until your operating system runs out of memory)
+ __Int:__ finite precission 32-bits
