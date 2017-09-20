# Kazu_R-club_Aug30
Kazu  
9/6/2017  



# 21.5 The map functions


```r
map_dbl(df, mean)
```

```
## Error: `.x` is not a vector (closure)
```

```r
map_dbl(df, median)
```

```
## Error: `.x` is not a vector (closure)
```

```r
map_dbl(df, sd)
```

```
## Error: `.x` is not a vector (closure)
```

```r
df %>% map_dbl(mean)
```

```
## Error: `.x` is not a vector (closure)
```
# 21.5.1 Shortcuts

```r
models <- mtcars %>% 
  split(.$cyl) %>% 
  map(function(df) lm(mpg ~ wt, data = df))
models
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
# shortcut "."
models <- mtcars %>% 
  split(.$cyl) %>% 
  map(~lm(mpg ~ wt, data = .))
models
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
# 
models %>% 
  map(summary) %>% 
  map_dbl(~.$r.squared) 
```

```
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
# cf
test$`4`$r.squared
```

```
## Error in eval(expr, envir, enclos): object 'test' not found
```

```r
# extracting named components is a common operation
models %>% 
  map(summary) %>% 
  map_dbl("r.squared")
```

```
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
# You can also use an integer to select elements by position:
x <- list(list(1, 2, 3), list(4, 5, 6), list(7, 8, 9))
x %>% map_dbl(2)
```

```
## [1] 2 5 8
```
# 21.5.2 Base R

```r
#lapply() is basically identical to map(), except that map() is consistent with all the other functions in purrr, and you can use the shortcuts for .f.

#Base sapply() is a wrapper around lapply() that automatically simplifies the output. This is useful for interactive work but is problematic in a function because you never know what sort of output you’ll get:
x1 <- list(
  c(0.27, 0.37, 0.57, 0.91, 0.20),
  c(0.90, 0.94, 0.66, 0.63, 0.06), 
  c(0.21, 0.18, 0.69, 0.38, 0.77)
)
x2 <- list(
  c(0.50, 0.72, 0.99, 0.38, 0.78), 
  c(0.93, 0.21, 0.65, 0.13, 0.27), 
  c(0.39, 0.01, 0.38, 0.87, 0.34)
)

threshold <- function(x, cutoff = 0.8) x[x > cutoff]
x1 %>% sapply(threshold) %>% str()
```

```
## List of 3
##  $ : num 0.91
##  $ : num [1:2] 0.9 0.94
##  $ : num(0)
```

```r
#> List of 3
#>  $ : num 0.91
#>  $ : num [1:2] 0.9 0.94
#>  $ : num(0)
x2 %>% sapply(threshold) %>% str()
```

```
##  num [1:3] 0.99 0.93 0.87
```

```r
#>  num [1:3] 0.99 0.93 0.87

map(x2, threshold) %>% str()
```

```
## List of 3
##  $ : num 0.99
##  $ : num 0.93
##  $ : num 0.87
```

```r
# vapply() is a safe alternative to sapply() because you supply an additional argument that defines the type. The only problem with vapply() is that it’s a lot of typing: vapply(df, is.numeric, logical(1)) is equivalent to map_lgl(df, is.numeric). One advantage of vapply() over purrr’s map functions is that it can also produce matrices — the map functions only ever produce vectors.

# I focus on purrr functions here because they have more consistent names and arguments, helpful shortcuts, and in the future will provide easy parallelism and progress bars.
```
# 21.5.3 Exercises

```r
#1. Write code that uses one of the map functions to:
## 1. Compute the mean of every column in mtcars.
mtcars %>% map_dbl(mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

```r
## 2. Determine the type of each column in nycflights13::flights.
nycflights13::flights %>% map_chr(typeof)
```

```
##           year          month            day       dep_time sched_dep_time 
##      "integer"      "integer"      "integer"      "integer"      "integer" 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##       "double"      "integer"      "integer"       "double"    "character" 
##         flight        tailnum         origin           dest       air_time 
##      "integer"    "character"    "character"    "character"       "double" 
##       distance           hour         minute      time_hour 
##       "double"       "double"       "double"       "double"
```

```r
nycflights13::flights %>% map_chr(class) # Error: Result 19 is not a length 1 atomic vector
```

```
## Error: Result 19 is not a length 1 atomic vector
```

```r
nycflights13::flights %>% map(class) #no error
```

```
## $year
## [1] "integer"
## 
## $month
## [1] "integer"
## 
## $day
## [1] "integer"
## 
## $dep_time
## [1] "integer"
## 
## $sched_dep_time
## [1] "integer"
## 
## $dep_delay
## [1] "numeric"
## 
## $arr_time
## [1] "integer"
## 
## $sched_arr_time
## [1] "integer"
## 
## $arr_delay
## [1] "numeric"
## 
## $carrier
## [1] "character"
## 
## $flight
## [1] "integer"
## 
## $tailnum
## [1] "character"
## 
## $origin
## [1] "character"
## 
## $dest
## [1] "character"
## 
## $air_time
## [1] "numeric"
## 
## $distance
## [1] "numeric"
## 
## $hour
## [1] "numeric"
## 
## $minute
## [1] "numeric"
## 
## $time_hour
## [1] "POSIXct" "POSIXt"
```

```r
## 3. Compute the number of unique values in each column of iris.
iris %>% map_int(function(x) length(unique(x)))
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

```r
## 4.Generate 10 random normals for each of μ=−10 , 0 , 10 , and 100 .
mu<-c(-10,0,10,100)
mu %>% map(function(x) rnorm(10,x)) # or
```

```
## [[1]]
##  [1] -10.829004 -10.228845 -10.451062 -10.029060  -9.443216 -12.041044
##  [7]  -9.598622  -9.475527  -8.638227  -9.772169
## 
## [[2]]
##  [1]  0.2822984  1.2379165 -2.2176128  0.1744977 -0.3121596 -0.5530128
##  [7] -0.7908559 -1.8671170  1.7148667 -0.4027246
## 
## [[3]]
##  [1]  8.837979 10.539045  9.695274 10.058225 10.919844 10.308559  9.814492
##  [8]  8.746774  9.856136  8.612763
## 
## [[4]]
##  [1]  97.72144 100.82774 100.22176 100.32211  97.62185 100.62234 100.50917
##  [8]  99.14197 100.71627  99.76875
```

```r
mu %>% map(function(x) rnorm(10,mean=x))
```

```
## [[1]]
##  [1] -10.240884  -8.589311 -11.894273  -9.219635 -10.972832 -10.853162
##  [7] -12.663734  -9.147352  -9.644172 -10.553771
## 
## [[2]]
##  [1]  1.77702910 -0.39605833 -0.91148568  0.72257830  0.42045650
##  [6] -0.75571006  0.76285580 -0.96475167  1.46125200  0.06286068
## 
## [[3]]
##  [1] 10.156183 11.601387 10.214310 10.254721 10.893302  9.923814  9.130114
##  [8] 11.616764  8.605318  9.283982
## 
## [[4]]
##  [1]  98.38729  99.40568  99.90651  99.49349  99.28309  99.04733  99.80303
##  [8] 100.14934 100.98171  99.81290
```

```r
# 
map(mu, rnorm,n=10) # simplified one
```

```
## [[1]]
##  [1] -11.450235 -10.121640  -9.855711 -11.518453 -10.558657 -10.032961
##  [7]  -9.469364 -10.440958 -10.764400  -8.516622
## 
## [[2]]
##  [1] -0.8420776  0.2893592 -1.1589854 -0.2920552 -0.6038707  1.5974327
##  [7]  1.0139565  1.0763644 -0.9086289 -1.3502318
## 
## [[3]]
##  [1] 11.156197 10.655950  8.540614 10.574753 10.639359  8.692083  8.215173
##  [8] 10.812185  9.618877  9.335260
## 
## [[4]]
##  [1] 100.49586  99.74686 100.60229  98.53270  99.06653 100.66663 100.17678
##  [8] 101.21328  99.52776  99.09070
```

```r
#2. How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?
df2 <- tibble(
  a = rnorm(10),
  x = letters[1:10],
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
str(df2)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	10 obs. of  5 variables:
##  $ a: num  -1.607 -2.009 0.509 0.659 0.526 ...
##  $ x: chr  "a" "b" "c" "d" ...
##  $ b: num  -0.2503 -0.7366 2.0212 0.8594 0.0855 ...
##  $ c: num  -0.121 0.372 -0.6 0.548 0.682 ...
##  $ d: num  1.577 -0.979 -0.42 -0.305 0.713 ...
```

```r
df3<-df2
df3$x<-as.factor(df3$x)
attributes(str(df3))
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	10 obs. of  5 variables:
##  $ a: num  -1.607 -2.009 0.509 0.659 0.526 ...
##  $ x: Factor w/ 10 levels "a","b","c","d",..: 1 2 3 4 5 6 7 8 9 10
##  $ b: num  -0.2503 -0.7366 2.0212 0.8594 0.0855 ...
##  $ c: num  -0.121 0.372 -0.6 0.548 0.682 ...
##  $ d: num  1.577 -0.979 -0.42 -0.305 0.713 ...
```

```
## NULL
```

```r
# is factor?
df3 %>% map(is.factor) # only "x" column
```

```
## $a
## [1] FALSE
## 
## $x
## [1] TRUE
## 
## $b
## [1] FALSE
## 
## $c
## [1] FALSE
## 
## $d
## [1] FALSE
```

```r
#3. What happens when you use the map functions on vectors that aren’t lists? What does map(1:5, runif) do? Why?
map(1:5, runif) # with different numbe of observation
```

```
## [[1]]
## [1] 0.5556003
## 
## [[2]]
## [1] 0.6252319 0.8087144
## 
## [[3]]
## [1] 0.4114969 0.4416450 0.4571188
## 
## [[4]]
## [1] 0.6149921 0.3333936 0.6191582 0.1496587
## 
## [[5]]
## [1] 0.4850547 0.7794324 0.9682839 0.3738480 0.5106641
```

```r
map(1, runif)
```

```
## [[1]]
## [1] 0.7753717
```

```r
map(2, runif)
```

```
## [[1]]
## [1] 0.2694203 0.1039447
```

```r
map(5, runif) 
```

```
## [[1]]
## [1] 0.6982492 0.0228982 0.5119759 0.6933652 0.1825926
```

```r
#4. What does map(-2:2, rnorm, n = 5) do? Why? What does map_dbl(-2:2, rnorm, n = 5) do? Why?
map(-2:2, rnorm, n = 5) # mean is now variable. n is fixed to "5"
```

```
## [[1]]
## [1] -1.406187 -2.760559 -2.192003 -1.933289 -2.210673
## 
## [[2]]
## [1] -1.72982167  0.30138407 -0.06455585  0.13566677  0.84466956
## 
## [[3]]
## [1] -1.7084510 -1.4975180 -0.6874186 -0.2721006  1.5081102
## 
## [[4]]
## [1] 0.50761089 0.01288115 2.68012074 0.50327103 2.69089783
## 
## [[5]]
## [1] 2.645033 2.602410 1.445456 2.583816 1.182431
```

```r
#5. Rewrite map(x, function(df) lm(mpg ~ wt, data = df)) to eliminate the anonymous function.
mtcars %>% map(function(df) lm(mpg ~ wt, data = df)) # original, Why this does not work?
```

```
## Error in eval(predvars, data, env): numeric 'envir' arg not of length one
```

```r
# what is anonymous function? see text
mtcars %>% map(function(df) lm(mpg ~ wt, data = df)) # original
```

```
## Error in eval(predvars, data, env): numeric 'envir' arg not of length one
```

```r
mtcars %>% map(~lm(mpg ~ wt, data =.)) # does not work
```

```
## Error in eval(predvars, data, env): numeric 'envir' arg not of length one
```

```r
mtcars %>% 
  split(.$cyl) %>%   map(~lm(mpg ~ wt, data = .)) # works.. becasue split(,.$cyl) gave list object (map() needs list)
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
#
mtcars %>% map(~lm(mpg ~ wt, data = .)) # does not work.. becasue split(,.$cyl) gave list object (map() needs 
```

```
## Error in eval(predvars, data, env): numeric 'envir' arg not of length one
```

```r
list(mtcars) %>% map(~lm(mpg ~ wt, data = .)) # does  work.. becasue split(,.$cyl) gave list object (map() needs 
```

```
## [[1]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      37.285       -5.344
```

```r
mtcars %>% map(~lm(mpg ~ wt, data = .)) # does not work.. becasue split(,.$cyl) gave list object (map() needs list)
```

```
## Error in eval(predvars, data, env): numeric 'envir' arg not of length one
```

```r
map(~lm(mpg ~ wt, data = .)) # works..
```

```
## Error in as_function(.f, ...): argument ".f" is missing, with no default
```
# 21.6 Dealing with failure

```r
safe_log <- safely(log)
str(safe_log(10))
```

```
## List of 2
##  $ result: num 2.3
##  $ error : NULL
```

```r
str(safe_log("a"))
```

```
## List of 2
##  $ result: NULL
##  $ error :List of 2
##   ..$ message: chr "non-numeric argument to mathematical function"
##   ..$ call   : language .f(...)
##   ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
```

```r
# 
x <- list(1, 10, "a")
y <- x %>% map(safely(log))
str(y)
```

```
## List of 3
##  $ :List of 2
##   ..$ result: num 0
##   ..$ error : NULL
##  $ :List of 2
##   ..$ result: num 2.3
##   ..$ error : NULL
##  $ :List of 2
##   ..$ result: NULL
##   ..$ error :List of 2
##   .. ..$ message: chr "non-numeric argument to mathematical function"
##   .. ..$ call   : language .f(...)
##   .. ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
```

```r
#
y <- y %>% transpose()
str(y)
```

```
## List of 2
##  $ result:List of 3
##   ..$ : num 0
##   ..$ : num 2.3
##   ..$ : NULL
##  $ error :List of 3
##   ..$ : NULL
##   ..$ : NULL
##   ..$ :List of 2
##   .. ..$ message: chr "non-numeric argument to mathematical function"
##   .. ..$ call   : language .f(...)
##   .. ..- attr(*, "class")= chr [1:3] "simpleError" "error" "condition"
```

```r
#
is_ok <- y$error %>% map_lgl(is_null)
x[!is_ok]
```

```
## [[1]]
## [1] "a"
```

```r
#
y$result[is_ok] %>% flatten_dbl() # unlist can not control output form vs flatten does
```

```
## [1] 0.000000 2.302585
```

```r
#
x <- list(1, 10, "a")
x %>% map_dbl(possibly(log, NA_real_))
```

```
## [1] 0.000000 2.302585       NA
```

```r
#
x <- list(1, -1)
x %>% map(quietly(log)) %>% str()
```

```
## List of 2
##  $ :List of 4
##   ..$ result  : num 0
##   ..$ output  : chr ""
##   ..$ warnings: chr(0) 
##   ..$ messages: chr(0) 
##  $ :List of 4
##   ..$ result  : num NaN
##   ..$ output  : chr ""
##   ..$ warnings: chr "NaNs produced"
##   ..$ messages: chr(0)
```
# 21.7 Mapping over multiple arguments

```r
mu <- list(5, 10, -3)
mu %>% 
  map(rnorm, n = 5) %>% 
  str()
```

```
## List of 3
##  $ : num [1:5] 6.02 4.53 5.45 5.7 3.63
##  $ : num [1:5] 9.46 11.05 10.09 10.77 10.11
##  $ : num [1:5] -3.92 -3.48 -2.98 -3.57 -2.4
```

```r
#
sigma <- list(1, 5, 10)
seq_along(mu) %>% 
  map(~rnorm(5, mu[[.]], sigma[[.]])) %>% 
  str()
```

```
## List of 3
##  $ : num [1:5] 4.78 4.67 5.27 3.88 3.91
##  $ : num [1:5] 2.56 14.43 17.15 7.45 3.15
##  $ : num [1:5] -6.15 -4.48 -1.58 -27.41 -15.9
```

```r
#
map2(mu, sigma, rnorm, n = 5) %>% str()
```

```
## List of 3
##  $ : num [1:5] 4.45 4.62 3.82 3.89 4.36
##  $ : num [1:5] 14.92 4.17 15.75 -4.91 13.97
##  $ : num [1:5] -5.29 10.67 -8.18 -8.03 -30.89
```

```r
#
map2 <- function(x, y, f, ...) {
  out <- vector("list", length(x))
  for (i in seq_along(x)) {
    out[[i]] <- f(x[[i]], y[[i]], ...)
  }
  out
}
# usage of pmap() 
n <- list(1, 3, 5)
args1 <- list(n, mu, sigma)
args1 %>%
  pmap(rnorm) %>% 
  str()
```

```
## List of 3
##  $ : num 5.44
##  $ : num [1:3] 25.11 16.01 3.85
##  $ : num [1:5] 1.3 -5.97 -20.83 10.95 -34.08
```

```r
# you can name args2
args2 <- list(mean = mu, sd = sigma, n = n)
args2 %>% 
  pmap(rnorm) %>% 
  str()
```

```
## List of 3
##  $ : num 4.83
##  $ : num [1:3] 10.6 13.9 12.5
##  $ : num [1:5] 5.87 -13 -10.05 5.91 6.49
```

```r
#
params <- tribble(
  ~mean, ~sd, ~n,
    5,     1,  1,
   10,     5,  3,
   -3,    10,  5
)
params %>% 
  pmap(rnorm)
```

```
## [[1]]
## [1] 4.821788
## 
## [[2]]
## [1] 13.51328 18.55762 14.95441
## 
## [[3]]
## [1]  -6.345541  -5.672127 -10.097694   8.025715   6.945374
```
# 21.7.1 Invoking different functions

```r
f <- c("runif", "rnorm", "rpois")
param <- list(
  list(min = -1, max = 1), 
  list(sd = 5), 
  list(lambda = 10)
)
#
invoke_map(f, param, n = 5) %>% str()
```

```
## List of 3
##  $ : num [1:5] 0.4963 0.6492 -0.6461 -0.7236 0.0774
##  $ : num [1:5] 2.62 12.4 -0.28 -4.88 -3.88
##  $ : int [1:5] 9 10 7 7 17
```

```r
#
sim <- tribble(
  ~f,      ~params,
  "runif", list(min = -1, max = 1),
  "rnorm", list(sd = 5),
  "rpois", list(lambda = 10)
)
sim %>% 
  mutate(sim = invoke_map(f, params, n = 10))
```

```
## # A tibble: 3 × 3
##       f     params        sim
##   <chr>     <list>     <list>
## 1 runif <list [2]> <dbl [10]>
## 2 rnorm <list [1]> <dbl [10]>
## 3 rpois <list [1]> <int [10]>
```

```r
#
```
# 21.8 Walk

```r
x <- list(1, "a", 3)

x %>%   walk(print)
```

```
## [1] 1
## [1] "a"
## [1] 3
```
