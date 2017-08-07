
THIS IS WORK IN PROGRESS.
THIS IS HEAVILLY INSPIRED ON UNIVERISTY OF MELBOURNE'S SUBJECT ON MODELS OF COMPUTATION (COMP30026), give credit to them.

# Characteristics of Haskell
+ Strong Type Language
+ Compiled (e.g of compiler ghc)
+ Extension of files is .hs
+ Indentation matters and determines scoping
+ A note on parenthesis:
    + Parenthesis in Haskell have the job of organizing how expressions are executed
    + Parenthesis CANNOT be used to pass in arguments to a function 
    + `mod 2016 100` means `2016` and `100` are arguments for mod
    + Given that `mysum` is a function that takes 2 int arguments and adds them up, `mysum (1+4) 5` means: Eval `1+4` and use the result.
    +  Given that `search` is a function that gets 2 arguments: the first argument is a function and the second is a list
        +  `search tst xs` means: `tst` and `xs` are arguments for search.  This will be ok according to `search` definition
        +  `search(tst xs)` means: eval `tst` with `xs` as argument and use the answer as a single argument for `search`.  This will most likely __blow up__ because because you are passing the wrong arguments to both `tst` and `search` functions.
+ ghci -> Interactive environment for Haskell
+ It is possible to run a Haskell program as a standalone executable if the special `main` function is defined.
+ There is no native concept of sequence, branching or loops.
+ A Haskell program is one big expression to be evaluated with determined inputs.
    + Small-scale functions get combined to form larger-scale functions. Larger-scale functions are combined in the same way to form even larger functions, until eventually we have a complete program.
    + A simple way to combine functions is to take the result of one function application and make that the input argument of another. 
* By convention, CamelCase is used for multiple words.

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
+ In pattern matching the `_` is used to match everything without giving importance to what the argument has (it can't be used in the expression).  For example `myFunc _ 0 =  error "bla bla"` means that no matter what the first argument is, if the second argument is 0, then `error: "bla bla"`
    + `myFunc _ 0` is equivalent to `myFunc x 0`; however, the use of a `_` is used to indicate others that the first argument should not be used on the expression for that pattern.

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
+ Store elements __of the same type.__ (If you don't want this, use a tuple)
+ Lists are defined entirely by recursion
    + `x:list` defines a list where `x` is the first element and `list` is the rest of the list (which is itself a list). 
        + The `:` is an _infix_ function that gets an element and a list and returns a list.
        + e.g `'h':('e':('l':('l':('o':[]))))`  or `h:e:l:l:o:[]`  
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
* Haskell lists do not provide random access, most list functions and operations [(including `length` and `!!`)](#pre-defined-list-functions) run in linear time in the length of the list.
* Haskell does support an array data structure providing random access, but we won't have a need for it.

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
-- base case
search x [] = False
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
+ Use where inside a function (as opposed to declaring a global function a using inside other function) when the inner (where) function does not mean anything for the global scope.
```haskell
maximum :: [Int] -> Int
-- maximum [x] is a pattern to match a list of a single element
maximum [x] = x
maximum (x:xs)
    | x > maxxs = x
    | otherwise = maxxs
    where 
        maxxs = maximum xs
        -- Other helper expresions (e.g minxs = minimum xs )
```

# Pre-defined list functions
There are some common use functions already defined by Haskell.
+ `sum`, `maximum`, `length`, `product`
+ `elem 3 [2,4,5]` returns `True` if the first argument is contained on the list.
+ `null` returns `True` if a list is empty, `False` otherwise.
+ `[1,2,3,4] !! 2` returns the nth element in a list (return 3 in this case)
+ The `++` operator joins to lists together `[1,2] ++ [3,4]`.
+ `head` and `tail` separate a list into its first and remaining elements:
```haskell
     1  [2,  3,  4,  5,  6,  7,  8,  9,  10]
head --'   <--------------tail-------------->
```
* `last` and `init` do just the opposite:
```haskell
[1,  2,  3,  4,  5,  6,  7,  8,  9]  10
 <---------------init------------>   '-- last
```
* `take n` and `drop n` split the list at an arbitrary point. For example, using 4 for n:
```haskell
[1,  2,  3,  4] [5,  6,  7,  8,  9,  10]
 <--take 4--->   <-------drop 4------->
```

* `filter` is a very useful pre-defined [higher-order function](#higher-order-functions). Takes in a _list_ and a _test function_ and filters out all of the elements for wich the _test function_ doesn't return __true__.
* `map` is another very useful pre-defined [higher-order function](#higher-order-functions). Takis in a _list_ and a _function to apply_ and applies it to every element of the list, returning a new list.

```haskell
-- Yes.. this is quicksort in 5 lines of code
qsort :: [Int] -> [Int]
qsort [] = []
qsort (pivot:others) = (qsort lowers) ++ [pivot] ++ (qsort highers)
    where lowers  = filter (<pivot)  others
          highers = filter (>=pivot) others
```

## Pre-defined functions included in the `Data.List` Haskell Module
* Haskell functions are collected and packaged in modules. The module that is loaded by default when you start a Haskell program is called the _Prelude_. The _Prelude_ contains definitions for everything that is provided by default for you: everything from `Int` and `+` to `maximum` and `filter`.
* To use this functions you need to `import Data.List`.
*  Some important functions are: 
    * `sort`
    * `group` separates a list into sublists of adjacent, matching numbers.
```haskell
 group [1,1,3,3,2,1]
 [[1,1],[3,3],[2],[1]]
```

    * `nub` removes duplicates
```haskell
nub [1,1,3,3,2,1]
[1,3,2]
```
    
    * `delete x mylist` removes the first occurence of x of _mylist_ 
```haskell
delete 3 [2,3,4,3]
[2,4,3]
```
    
    - The `\\` operator is an _infix_ operator that deletes each element of the right list from the left list (each element is deleted once, see example)
```haskell
[1,2,3,2] \\ [2,1] 
[3,2]
```

* You can use these functions as building blocks to define other useful functions.
```haskell
dups :: [Int] -> [Int]
dups xs = xs \\ (nub xs)

dups [1,1,2,3]
[1]

groupAll :: [Int] -> [[Int]]
groupAll xs = group (sort xs)

groupAll[1,2,3,1,2,3]
[[1,1],[2,2],[3,3]]

-- as opposed to 
group[1,2,3,1,2,3]
[[1],[2],[3],[1],[2],[3]]
```


# Higher-order functions
* Functions and recieve other functions as arguments or can return functions.
* Functions that recieve other functions as arguments are called higher-order functions.

## Higher-order functions example
This concept is better explained through an example:
A higher-order search function would allow us to provide the function that decides when to stop searching as an argument.

For example, lets say we had the following helper functions available (Note that both functions are of type `Ìnt -> Bool`):

```haskell
evn :: Int -> Bool
evn x = (x `mod` 2 == 0)
​
big :: Int -> Bool
big x = (x > 100)
```

It'd be great if we could pass these functions as arguments to our search function. Something like this:
```haskell
Main> search evn [1, 2, 3, 4, 5]
2
Main> search big [1, 2, 101, 4, 5]
101
Main> search evn [1, 3, 5, 7, 9]
error: search: no matching item found 
```

So what we want is a search function that takes two arguments:

+ a function which can test each integer in the list and tell us True if there's a match. Its type should be Int -> Bool. Let's call this argument tst.
+ a list of integers to search through, of type [Int]

```haskell
search :: (Int -> Bool) -> [Int] -> Int
search tst [] = error "search: no matching number found"
search tst (x:xs)
    | tst x     = x
    | otherwise = search tst xs
```
__Notice the parenthesis on the first argument of the function `(Int -> Bool)`.__ This indicates that the first argument is of type `Int -> Bool`. No parenthesis will cause the search function to have 3 input arguments `Int, Bool and [Int]`

# Using the ghci console
+ :t gives the type of whatever is given as argument
```haskell
-- given g
g :: Int -> Int -> Int
g x y = x * y
-- then 
:t g
-- #=> g :: Int -> Int -> Int
```

# Tuples
+ A tuple lets you store a fixed number of values of different (but fixed) types

# Types
+ Haskell is strongly typed.
+ All type errors are caught at compilation time due to it's powerfull type inference engine.
+ Types: Int, Integer, Bool, Char, String, Float, Double, [...], [[...]].
    +   The last 2 mean a list of any other type and a list of lists of any other type.
+ __Integer:__ infinite precision integer type (Store any number, however large (well, until your operating system runs out of memory)
+ __Int:__ finite precission 32-bits
+ Thinkd of `[]` _lists_  and `()` _tuples_ as type constructors.
    +  e.g `[Int]` builds a new type _list of integers._
-  __String__ is a type _synonym_ for `[Char]`. Both ways of defining ir are interchangable.
    - You can apply all list functions to a `String` because it is a list of chars.   
* __Defining type synonyms__
    - `type Pair = (Int,Int)
* __Polymorphism__
    * In the following example `a` is a _type variable_. It means `a` is of any type.   
```haskell
len :: [a] -> Int
len []     = 0
len (x:xs) = 1 + len xs
```
