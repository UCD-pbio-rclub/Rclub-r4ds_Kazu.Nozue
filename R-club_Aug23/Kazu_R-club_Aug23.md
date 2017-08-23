# Kazu_Aug23
Kazu  
8/22/2017  


# 19.5 Function arguments

```r
#Notice that when you call a function, you should place a space around = in function calls, and always put a space after a comma, not before (just like in regular English). Using whitespace makes it easier to skim the function for the important components.

# Good
average <- mean(feet / 12 + inches, na.rm = TRUE)
```

```
## Error in mean(feet/12 + inches, na.rm = TRUE): object 'feet' not found
```

```r
# Bad
average<-mean(feet/12+inches,na.rm=TRUE)
```

```
## Error in mean(feet/12 + inches, na.rm = TRUE): object 'feet' not found
```
# 19.5.1 Choosing names
# 19.5.2 Checking values

# 19.5.3 Dot-dot-dot (…)

```r
commas <- function(...) stringr::str_c(..., collapse = ", ")
commas(letters[1:10])
```

```
## [1] "a, b, c, d, e, f, g, h, i, j"
```

```r
#> [1] "a, b, c, d, e, f, g, h, i, j"

rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}
rule("Important output")
```

```
## Important output ------------------------------------------------------
```

```r
#
x <- c(1, 2)
sum(x, na.mr = TRUE) # typo. 4
```

```
## [1] 4
```

```r
sum(x, na.rm = TRUE) # no type. 3
```

```
## [1] 3
```
# 19.5.4 Lazy evaluation
# 19.5.5 Exercises

```r
#1. What does commas(letters, collapse = "-") do? Why?
commas(letters, collapse = "-")
```

```
## Error in stringr::str_c(..., collapse = ", "): formal argument "collapse" matched by multiple actual arguments
```

```r
# Error in stringr::str_c(..., collapse = ", ") : 
#  formal argument "collapse" matched by multiple actual arguments

commas <- function(...) stringr::str_c(..., collapse = "-")
commas(letters, collapse = "-") # does not work
```

```
## Error in stringr::str_c(..., collapse = "-"): formal argument "collapse" matched by multiple actual arguments
```

```r
commas <- function(...,collapse=", ") stringr::str_c(..., collapse = collapse)
commas(letters, collapse = "-")
```

```
## [1] "a-b-c-d-e-f-g-h-i-j-k-l-m-n-o-p-q-r-s-t-u-v-w-x-y-z"
```

```r
#2. It’d be nice if you could supply multiple characters to the pad argument, e.g. rule("Title", pad = "-+"). Why doesn’t this currently work? How could you fix it?
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}
rule("Title", pad = "-+")
```

```
## Title -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

```r
rule("Title", pad = "-+*") 
```

```
## Title -+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*-+*
```

```r
# what does author want me to do? split pad into each character?


#3. What does the trim argument to mean() do? When might you use it?
?mean

x <- c(0:10, 50)
xm <- mean(x)
c(xm, mean(x, trim = 0.10))
```

```
## [1] 8.75 5.50
```

```r
mean(c(x,50), trim = 0.10)
```

```
## [1] 9.545455
```

```r
mean(c(x,100), trim = 0.10)
```

```
## [1] 9.545455
```

```r
# 
mean(c(0:10,100:103), trim = 0.1)
```

```
## [1] 27.53846
```

```r
mean(c(0:5,100:103,6:10), trim = 0.1)
```

```
## [1] 27.53846
```

```r
#4. The default value for the method argument to cor() is c("pearson", "kendall", "spearman"). What does that mean? What value is used by default?
?cor
# choices of methods 
# "pearson" (default)
```
#19.6 Return values
# 19.6.1 Explicit return statements

# 19.6.2 Writing pipeable functions

```r
# If you want to write your own pipeable functions, it’s important to think about the return value. Knowing the return value’s object type will mean that your pipeline will “just work”. For example, with dplyr and tidyr the object type is the data frame.

# There are two basic types of pipeable functions: transformations and side-effects. With transformations, an object is passed to the function’s first argument and a modified object is returned. With side-effects, the passed object is not transformed. Instead, the function performs an action on the object, like drawing a plot or saving a file. Side-effects functions should “invisibly” return the first argument, so that while they’re not printed they can still be used in a pipeline. For example, this simple function prints the number of missing values in a data frame:
show_missings <- function(df) {
  n <- sum(is.na(df))
  cat("Missing values: ", n, "\n", sep = "")
  
  invisible(df)
}
# If we call it interactively, the invisible() means that the input df doesn’t get printed out:
show_missings(mtcars)
```

```
## Missing values: 0
```

```r
# But it’s still there, it’s just not printed by default:
x <- show_missings(mtcars) 
```

```
## Missing values: 0
```

```r
#> Missing values: 0
class(x)
```

```
## [1] "data.frame"
```

```r
#> [1] "data.frame"
dim(x)
```

```
## [1] 32 11
```

```r
#> [1] 32 11
# And we can still use it in a pipe:
mtcars %>% 
  show_missings() %>% 
  mutate(mpg = ifelse(mpg < 20, NA, mpg)) %>% 
  show_missings() 
```

```
## Missing values: 0
## Missing values: 18
```

```r
## (MO) should be invisible()? 
show_missings <- function(df) {
  n <- sum(is.na(df))
  cat("Missing values: ", n, "\n", sep = "")
  return(df) # needs this for pipe above
  #invisible(df)
}
```
# 19.7 Environment

# chapter 20: Vectors
## 20.1 Introduction
## 20.2 Vector basics

```r
# There are two types of vectors:
# Atomic vectors, of which there are six types: logical, integer, double, character, complex, and raw. Integer and double vectors are collectively known as numeric vectors.

# Lists, which are sometimes called recursive vectors because lists can contain other lists.

# The chief difference between atomic vectors and lists is that atomic vectors are homogeneous, while lists can be heterogeneous. There’s one other related object: NULL. NULL is often used to represent the absence of a vector (as opposed to NA which is used to represent the absence of a value in a vector). NULL typically behaves like a vector of length 0. Figure 20.1 summarises the interrelationships.
```
# 20.3 Important types of atomic vector
# 20.3.1 Logical

```r
1:10 %% 3 == 0
```

```
##  [1] FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE
```
# 20.3.2 Numeric

```r
typeof(1)
```

```
## [1] "double"
```

```r
#> [1] "double"
typeof(1L) # To make an integer, place an L after the number:
```

```
## [1] "integer"
```

```r
#> [1] "integer"
1.5L
```

```
## [1] 1.5
```

```r
#> [1] 1.5
#Warning message:
#integer literal 1.5L contains decimal; using numeric value 
```
# 20.3.3 Character

```r
#This reduces the amount of memory needed by duplicated strings. You can see this behaviour in practice with pryr::object_size():

x <- "This is a reasonably long string."
pryr::object_size(x)
```

```
## 136 B
```

```r
#> 136 B

y <- rep(x, 1000)
pryr::object_size(y)
```

```
## 8.13 kB
```

```r
#> 8.13 kB
#y doesn’t take up 1,000x as much memory as x, because each element of y is just a pointer to that same string. A pointer is 8 bytes, so 1000 pointers to a 136 B string is 8 * 1000 + 136 = 8.13 kB.
```
# 20.3.4 Missing values

# 20.3.5 Exercises

```r
#1. Describe the difference between is.finite(x) and !is.infinite(x).
?is.finite()
?is.infinite()
x<-c(Inf,0:8,-Inf,0.00,NA,NaN)
is.finite(x) # Inf, -Inf, NA, NaN are FALSE
```

```
##  [1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
## [12]  TRUE FALSE FALSE
```

```r
is.infinite(x) # Inf, -Inf are TRUE
```

```
##  [1]  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
## [12] FALSE FALSE FALSE
```

```r
!is.infinite(x) # Inf, -Inf are FALSE, while NA, NaN are TRUE.
```

```
##  [1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
## [12]  TRUE  TRUE  TRUE
```

```r
#2. Read the source code for dplyr::near() (Hint: to see the source code, drop the ()). How does it work?
dplyr::near
```

```
## function (x, y, tol = .Machine$double.eps^0.5) 
## {
##     abs(x - y) < tol
## }
## <environment: namespace:dplyr>
```

```r
# function (x, y, tol = .Machine$double.eps^0.5) 
# {
#     abs(x - y) < tol
# }
?.Machine
.Machine
```

```
## $double.eps
## [1] 2.220446e-16
## 
## $double.neg.eps
## [1] 1.110223e-16
## 
## $double.xmin
## [1] 2.225074e-308
## 
## $double.xmax
## [1] 1.797693e+308
## 
## $double.base
## [1] 2
## 
## $double.digits
## [1] 53
## 
## $double.rounding
## [1] 5
## 
## $double.guard
## [1] 0
## 
## $double.ulp.digits
## [1] -52
## 
## $double.neg.ulp.digits
## [1] -53
## 
## $double.exponent
## [1] 11
## 
## $double.min.exp
## [1] -1022
## 
## $double.max.exp
## [1] 1024
## 
## $integer.max
## [1] 2147483647
## 
## $sizeof.long
## [1] 8
## 
## $sizeof.longlong
## [1] 8
## 
## $sizeof.longdouble
## [1] 16
## 
## $sizeof.pointer
## [1] 8
```

```r
noquote(unlist(format(.Machine)))
```

```
##            double.eps        double.neg.eps           double.xmin 
##          2.220446e-16          1.110223e-16         2.225074e-308 
##           double.xmax           double.base         double.digits 
##         1.797693e+308                     2                    53 
##       double.rounding          double.guard     double.ulp.digits 
##                     5                     0                   -52 
## double.neg.ulp.digits       double.exponent        double.min.exp 
##                   -53                    11                 -1022 
##        double.max.exp           integer.max           sizeof.long 
##                  1024            2147483647                     8 
##       sizeof.longlong     sizeof.longdouble        sizeof.pointer 
##                     8                    16                     8
```

```r
# tol in dplyr:near is Tolerance of comparison.
# double.eps	
# the smallest positive floating-point number x such that 1 + x != 1. It equals double.base ^ ulp.digits if either double.base is 2 or double.rounding is 0; otherwise, it is (double.base ^ double.ulp.digits) / 2. Normally 2.220446e-16.

sqrt(2) ^ 2 == 2
```

```
## [1] FALSE
```

```r
near(sqrt(2) ^ 2, 2)
```

```
## [1] TRUE
```

```r
# This is a safe way of comparing if two vectors of floating point numbers are (pairwise) equal. This is safer than using ==, because it has a built in tolerance


#3. A logical vector can take 3 possible values. How many possible values can an integer vector take? How many possible values can a double take? Use google to do some research.
#?
#4. Brainstorm at least four functions that allow you to convert a double to an integer. How do they differ? Be precise.

x<-c(3.49999,4.4999999,4.50000000001)
typeof(x) # double
```

```
## [1] "double"
```

```r
as.integer(x) # 3 4 4
```

```
## [1] 3 4 4
```

```r
round(x) #3 4 5
```

```
## [1] 3 4 5
```

```r
ceiling(x) # 4 5 5 
```

```
## [1] 4 5 5
```

```r
floor(x) #3 4 4
```

```
## [1] 3 4 4
```

```r
#5. What functions from the readr package allow you to turn a string into logical, integer, and double vector?
library(readr)
# under search..... #
parse_integer(c("1", "2", "3"))
```

```
## [1] 1 2 3
```

```r
parse_double(c("1", "2", "3.123"))
```

```
## [1] 1.000 2.000 3.123
```

```r
parse_number("$1,123,456.00")
```

```
## [1] 1123456
```

```r
parse_logical(1:10)
```

```
## Warning: 9 parsing failures.
## row col           expected actual
##   2  -- 1/0/T/F/TRUE/FALSE      2
##   3  -- 1/0/T/F/TRUE/FALSE      3
##   4  -- 1/0/T/F/TRUE/FALSE      4
##   5  -- 1/0/T/F/TRUE/FALSE      5
##   6  -- 1/0/T/F/TRUE/FALSE      6
## ... ... .................. ......
## See problems(...) for more details.
```

```
##  [1] TRUE   NA   NA   NA   NA   NA   NA   NA   NA   NA
## attr(,"problems")
## # A tibble: 9 × 4
##     row   col           expected actual
##   <int> <int>              <chr>  <chr>
## 1     2    NA 1/0/T/F/TRUE/FALSE      2
## 2     3    NA 1/0/T/F/TRUE/FALSE      3
## 3     4    NA 1/0/T/F/TRUE/FALSE      4
## 4     5    NA 1/0/T/F/TRUE/FALSE      5
## 5     6    NA 1/0/T/F/TRUE/FALSE      6
## 6     7    NA 1/0/T/F/TRUE/FALSE      7
## 7     8    NA 1/0/T/F/TRUE/FALSE      8
## 8     9    NA 1/0/T/F/TRUE/FALSE      9
## 9    10    NA 1/0/T/F/TRUE/FALSE     10
```

```r
parse_logical(TRUE)
```

```
## [1] TRUE
```

```r
col_logical()
```

```
## <collector_logical>
```
