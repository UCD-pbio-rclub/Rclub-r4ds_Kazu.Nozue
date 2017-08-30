# Kazu_R-club_Aug30
Kazu  
8/30/2017  



# 20.4 Using atomic vectors
## 20.4.1 Coercion

```r
typeof(c(TRUE, 1L))
```

```
## [1] "integer"
```

```r
#> [1] "integer"
typeof(c(1L, 1.5))
```

```
## [1] "double"
```

```r
#> [1] "double"
typeof(c(1.5, "a"))
```

```
## [1] "character"
```

```r
#> [1] "character"
```
## 20.4.2 Test functions

```r
x <- sample(20, 100, replace = TRUE)
?is_logical()
is_logical(x)
```

```
## [1] FALSE
```

```r
is_integer()
```

```
## Error in typeof(x): argument "x" is missing, with no default
```
## 20.4.3 Scalars and recycling rules

```r
sample(10) + 100
```

```
##  [1] 102 103 107 101 108 106 104 109 105 110
```

```r
runif(10) > 0.5
```

```
##  [1]  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE
```

```r
1:10 + 1:2
```

```
##  [1]  2  4  4  6  6  8  8 10 10 12
```

```r
1:10 + 1:3
```

```
## Warning in 1:10 + 1:3: longer object length is not a multiple of shorter
## object length
```

```
##  [1]  2  4  6  5  7  9  8 10 12 11
```

```r
tibble(x = 1:4, y = 1:2) # error
```

```
## Error: Variables must be length 1 or 4.
## Problem variables: 'y'
```

```r
tibble(x = 1:4, y = rep(1:2, 2))
```

```
## # A tibble: 4 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     2     2
## 3     3     1
## 4     4     2
```

```r
tibble(x = 1:4, y = rep(1:2, each = 2))
```

```
## # A tibble: 4 × 2
##       x     y
##   <int> <int>
## 1     1     1
## 2     2     1
## 3     3     2
## 4     4     2
```
## 20.4.4 Naming vectors

```r
c(x = 1, y = 2, z = 4)
```

```
## x y z 
## 1 2 4
```

```r
purrr::set_names(1:3, c("a", "b", "c"))
```

```
## a b c 
## 1 2 3
```
## 20.4.5 Subsetting

```r
x <- c("one", "two", "three", "four", "five")
x[-1]
```

```
## [1] "two"   "three" "four"  "five"
```

```r
x[-1,3] # It’s an error to mix positive and negative values:
```

```
## Error in x[-1, 3]: incorrect number of dimensions
```

```r
x <- c(10, 3, NA, 5, 8, 1, NA)

# All non-missing values of x
x[!is.na(x)]
```

```
## [1] 10  3  5  8  1
```

```r
#> [1] 10  3  5  8  1

# All even (or missing!) values of x
x[x %% 2 == 0]
```

```
## [1] 10 NA  8 NA
```
## 20.4.6 Exercises

```r
#1. What does mean(is.na(x)) tell you about a vector x? What about sum(!is.finite(x))?
x <- c(10, 3, NA, 5, 8, 1, NA)
mean(is.na(x)) # ratio of NA in x
```

```
## [1] 0.2857143
```

```r
sum(!is.finite(x)) # sum of numbers of not finite (= two "NA")
```

```
## [1] 2
```

```r
#2. Carefully read the documentation of is.vector(). What does it actually test for? Why does is.atomic() not agree with the definition of atomic vectors above?
?is.vector
is.vector(x)
```

```
## [1] TRUE
```

```r
is.atomic(x) 
```

```
## [1] TRUE
```

```r
#3. Compare and contrast setNames() with purrr::set_names().
?setNames
setNames( 1:3, c("foo", "bar", "baz") )
```

```
## foo bar baz 
##   1   2   3
```

```r
setNames( 1:3) # Error in setNames(1:3) : argument "nm" is missing, with no default
```

```
## Error in setNames(1:3): argument "nm" is missing, with no default
```

```r
?purrr::set_names
purrr::set_names( 1:3, c("foo", "bar", "baz") )
```

```
## foo bar baz 
##   1   2   3
```

```r
purrr::set_names( 1:3)
```

```
## 1 2 3 
## 1 2 3
```

```r
#4. Create functions that take a vector as input and returns:
#4.1 The last value. Should you use [ or [[?
x
```

```
## [1] 10  3 NA  5  8  1 NA
```

```r
value_last<-function(x) {x[length(x)]}
value_last(x)
```

```
## [1] NA
```

```r
y<-sample(10,10)
y
```

```
##  [1]  1  7  9  6  8  4  5 10  3  2
```

```r
value_last(y)
```

```
## [1] 2
```

```r
#4.2 The elements at even numbered positions.
pos_even<-function(x) {x[x %% 2 ==0]}
y
```

```
##  [1]  1  7  9  6  8  4  5 10  3  2
```

```r
pos_even(y)
```

```
## [1]  6  8  4 10  2
```

```r
#4.3 Every element except the last value.
value_but_last<-function(x) {x[-length(x)]}
x
```

```
## [1] 10  3 NA  5  8  1 NA
```

```r
value_but_last(x)
```

```
## [1] 10  3 NA  5  8  1
```

```r
#4.4 Only even numbers (and no missing values).
pos_even_noNA<-function(x) {
  temp<-x[!is.na(x)]
  temp[temp %% 2 ==0]
}
x
```

```
## [1] 10  3 NA  5  8  1 NA
```

```r
pos_even_noNA(x)
```

```
## [1] 10  8
```

```r
#5. Why is x[-which(x > 0)] not the same as x[x <= 0]?
?which
x<-rnorm(100)
x[-which(x > 0)]
```

```
##  [1] -0.12659479 -0.58868433 -1.59806212 -0.61185060 -0.76178865
##  [6] -0.27541388 -0.15830726 -0.21539301 -0.59026884 -0.52477603
## [11] -2.61843043 -0.33719139 -0.13768640 -0.13998296 -0.12430142
## [16] -1.03286464 -0.33940608 -0.10894992 -1.06383553 -0.60278575
## [21] -2.01193605 -0.16970152 -0.64950480 -0.10303315 -0.42794911
## [26] -0.20033944 -0.47480391 -0.99508412 -1.78274500 -1.55180576
## [31] -0.86344341 -2.04810393 -0.44960748 -0.52130145 -0.24099148
## [36] -0.07517554 -0.47032668 -0.91442129 -1.23411994 -0.18853652
## [41] -0.12927211 -1.03781004 -0.05882007 -1.34708201 -2.10885868
## [46] -0.28206841 -0.41727239 -1.37608242 -0.32032501 -0.98073063
```

```r
x[x <= 0]
```

```
##  [1] -0.12659479 -0.58868433 -1.59806212 -0.61185060 -0.76178865
##  [6] -0.27541388 -0.15830726 -0.21539301 -0.59026884 -0.52477603
## [11] -2.61843043 -0.33719139 -0.13768640 -0.13998296 -0.12430142
## [16] -1.03286464 -0.33940608 -0.10894992 -1.06383553 -0.60278575
## [21] -2.01193605 -0.16970152 -0.64950480 -0.10303315 -0.42794911
## [26] -0.20033944 -0.47480391 -0.99508412 -1.78274500 -1.55180576
## [31] -0.86344341 -2.04810393 -0.44960748 -0.52130145 -0.24099148
## [36] -0.07517554 -0.47032668 -0.91442129 -1.23411994 -0.18853652
## [41] -0.12927211 -1.03781004 -0.05882007 -1.34708201 -2.10885868
## [46] -0.28206841 -0.41727239 -1.37608242 -0.32032501 -0.98073063
```

```r
identical(x[-which(x > 0)],x[x <= 0]) # true
```

```
## [1] TRUE
```

```r
#6. What happens when you subset with a positive integer that’s bigger than the length of the vector? What happens when you subset with a name that doesn’t exist?
xx <- c("one", "two", "three", "four", "five")
xx[10]
```

```
## [1] NA
```

```r
#> [1] NA
xx["ten"]
```

```
## [1] NA
```

```r
#> [1] NA
```
# 20.5 Recursive vectors (lists)
## 20.5.1 Visualising lists

```r
a <- list(a = 1:3, b = "a string", c = pi, d = list(-1, -5))
str(a[1:2])
```

```
## List of 2
##  $ a: int [1:3] 1 2 3
##  $ b: chr "a string"
```

```r
str(a[4])
```

```
## List of 1
##  $ d:List of 2
##   ..$ : num -1
##   ..$ : num -5
```

```r
str(a[[1]])
```

```
##  int [1:3] 1 2 3
```

```r
str(a[[4]])
```

```
## List of 2
##  $ : num -1
##  $ : num -5
```

```r
a$a
```

```
## [1] 1 2 3
```

```r
a[["a"]]
```

```
## [1] 1 2 3
```
## 20.5.3 Lists of condiments

## 20.5.4 Exercises

```r
#1. Draw the following lists as nested sets:
#1.1 list(a, b, list(c, d), list(e, f))
#1.2 list(list(list(list(list(list(a))))))

#2. What happens if you subset a tibble as if you’re subsetting a list? What are the key differences between a list and a tibble?
tb <- tibble::tibble(x = 1:5, y = 5:1)
typeof(tb)
```

```
## [1] "list"
```

```r
#> [1] "list"
attributes(tb)
```

```
## $names
## [1] "x" "y"
## 
## $class
## [1] "tbl_df"     "tbl"        "data.frame"
## 
## $row.names
## [1] 1 2 3 4 5
```

```r
tb[[1]]
```

```
## [1] 1 2 3 4 5
```

```r
names(tb)
```

```
## [1] "x" "y"
```
# 20.6 Attributes

```r
x <- 1:10
attr(x, "greeting")
```

```
## NULL
```

```r
#> NULL
attr(x, "greeting") <- "Hi!"
attr(x, "farewell") <- "Bye!"
attributes(x)
```

```
## $greeting
## [1] "Hi!"
## 
## $farewell
## [1] "Bye!"
```
# 20.7 Augmented vectors
## 20.7.1 Factors

## 20.7.2 Dates and date-times

## 20.7.3 Tibbles

```r
tb <- tibble::tibble(x = 1:5, y = 5:1)
typeof(tb)
```

```
## [1] "list"
```

```r
#> [1] "list"
attributes(tb)
```

```
## $names
## [1] "x" "y"
## 
## $class
## [1] "tbl_df"     "tbl"        "data.frame"
## 
## $row.names
## [1] 1 2 3 4 5
```

## 20.7.4 Exercises

```r
#1. What does hms::hms(3600) return? How does it print? What primitive type is the augmented vector built on top of? What attributes does it use?
hms::hms(3600) # 01:00:00
```

```
## 01:00:00
```

```r
hms::hms
```

```
## function (seconds = NULL, minutes = NULL, hours = NULL, days = NULL) 
## {
##     args <- list(seconds = seconds, minutes = minutes, hours = hours, 
##         days = days)
##     check_args(args)
##     arg_secs <- mapply(`*`, args, c(1, 60, 3600, 86400))
##     secs <- Reduce(`+`, arg_secs[vapply(arg_secs, length, integer(1L)) > 
##         0L])
##     as.hms(as.difftime(secs, units = "secs"))
## }
## <environment: namespace:hms>
```

```r
?hms
```

```
## Help on topic 'hms' was found in the following packages:
## 
##   Package               Library
##   lubridate             /Library/Frameworks/R.framework/Versions/3.3/Resources/library
##   hms                   /Library/Frameworks/R.framework/Versions/3.3/Resources/library
## 
## 
## Using the first match ...
```

```r
#2. Try and make a tibble that has columns with different lengths. What happens?
tibble(a=1:10,b=letters[1:5]) 
```

```
## Error: Variables must be length 1 or 10.
## Problem variables: 'b'
```

```r
# Error: Variables must be length 1 or 10.
# Problem variables: 'b'

#3. Based on the definition above, is it ok to have a list as a column of a tibble?
# I think so.
# needs to try
```
