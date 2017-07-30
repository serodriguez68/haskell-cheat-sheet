
THIS IS WORK IN PROGRESS.
THIS IS HEAVILLY INSPIRED ON UNIVERISTY OF MELBOURNE'S SUBJECT ON MODELS OF COMPUTATION (COMP30026)

# Characteristics of Haskell
+ Strong Type Language
+ Compiled (e.g of compiler ghc)
+ Extension of files is .hs
+ CHECK SERGIO: DOES INDENTATION MATTER?
+ CHECK SERGIO: USE OF PARENTHESIS? why does this doesn't work?
    + `mod(2016 100)`
    + `search(x y:ys)`
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
-- note that there is no = sign on the next line
fact n
    | n < 0     = 0
    | n == 0    = 1
    | otherwise = n * fact (n-1)
```

# Operators
+ Most operators are equal to operators in other programming languages
+ Noteworthy exceptions:
    - `/` provides float division, even if its arguments are both integers. For integer division (rounding down), use the `div` function, as in `div 16 3` (which will give 5).
    - `%` is not used as the 'modulo' operator. Use the `mod` function instead, as in `mod 16 3` (which will give 1)   
    - Using `-` as a unary negative sign (as in `-2`) rather than a subtraction operator (as in `5 - 2`) can cause problems. Always enclose such a negative number in brackets: `(-2)`.
    - `!` isn't for Boolean negation (not). Use the `not` function instead, as in `not True` (which will `give False`).
    - `!=` is not defined. For 'not equals', use the `/=` operator.
+ Operators are just functions that get their arguments from both sides of the function (this is called an _infix_ function as opposed to _prefix_ function).
+ To use an infix function (like +) as a prefix function, just enclose it in parentheses:
``` haskell
f :: Int -> Int -> Int
f x y = (+) x y
```
+ To use a prefix function (like div) as an infix function, just enclose it in backticks ('`' characters):
``` haskell
g :: Int -> Int -> Int
g x y = x `div` y
```
# Lists
+ Lists (i.e _linked lists_): most fundamentas data structure in Haskell
+ Lists are defined entirely by recursion
    + `x:list` defines a list where `x` is the first element and `list` is the rest of the list (which is itself a list). 
        + The `:` returns a list? CHECK SERGIO???? 
        + e.g `'h':('e':('l':('l':('o':[]))))`    
        + shorthand expression: `['h','e','l','l','o'] `
        + In the special case of character lists you can also write: `"hello"`
* There is a convention on list notation among Haskell programmers: if `x` is an element, `xs` is a list of such elements and `xss` is a list of lists. 
* Range notation:
    * `[1..6]` gives  `[1,2,3,4,5,6]`
    * `['a'..'e']` gives `['a','b','c','d','e']`
    * Step size by giving the first two elements and upper bound: `[1,3..6] gives [1,3,5]`
* Returning a list of certain type in a function
``` haskell
evens :: Int -> [Int]
evens 0 = []
evens n = [2,4..n*2]
```

# Recursion with lists
+ Recursive functions with lists as arguments often need to handle 2 cases:
    + Base cases: Handle the empty list and/or the single element list.
    + Handle the non-base case list recursively: process the first argument in the list an recursively process the rest of the list.
``` haskell
sum :: [Int] -> Int
-- Handle empty list
sum []     = 0
-- Recursively handle non-empty list
-- x is the first element, xs is the rest of the list
sum (x:xs) = x + sum xs
-- note the parenthesis in sum (x:xs), otherwise the expression would be interpreted as (sum x):xs = x + sum xs, which makes no sense
```

```haskell
search :: Int -> [Int] -> Bool
-- Base case 1: Handle empty list
search x [] = False
-- Base case 2: Handle one element list
search x [y] = (x == y)
-- Recursively handle all other lists
search x (y:ys)
    | x == y = True
    | otherwise = search_x_in_ys
    -- The where helper is explained in other section
    where search_x_in_ys = search x ys
```

# Helper expressions using `where`
+ Define variables and functions
+ Avoid repetition of code and computation (CHECK SERGIO: WHY AVOIDS REPETITION OF COMPUTATION?)
+ Use where when (CHECK SERGIO: when to use where instead of another function?)
```haskell
maximum :: [Int] -> Int
-- maximum [x] is a pattern to match a list of a single element
maximum [x] = x
maximum (x:xs)
    | x > maxxs = x
    | otherwise = maxxs
    where maxxs = maximum xs
```

# Pre-defined list functions
There are some common use functions already defined by Haskell.
+ `sum`, `maximum`, `length`
+ `elem 3 [2,4,5]` returns `True` if the first argument is contained on the list.
+ `null` returns `True` if a list is empty, `False` otherwise.
+ `[1,2,3,4] !! 2` returns the nth element in a list (return 3 in this case)
+ The `++` operator joins to lists together `[1,2] ++ [3,4]`.

# Types
+ __Integer:__ infinite precision integer type (Store any number, however large (well, until your operating system runs out of memory)
+ __Int:__ finite precission 32-bits
