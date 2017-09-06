# Kazu_R-club_Aug30
Kazu  
9/6/2017  


# 21 Iteration
## 21.2 For loops

```r
df <- tibble(
  a = rnorm(10),
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
# loop
output <- vector("double", ncol(df))  # 1. output
for (i in seq_along(df)) {            # 2. sequence
  output[[i]] <- median(df[[i]])      # 3. body
}
output
```

```
## [1]  0.1522769 -0.3899797  0.5608041  0.2732978
```

```r
## Note: "You might not have seen seq_along() before. It’s a safe version of the familiar 1:length(l), with an important difference: if you have a zero-length vector, seq_along() does the right thing:
y <- vector("double", 0)
seq_along(y)
```

```
## integer(0)
```

```r
#> integer(0)
1:length(y)
```

```
## [1] 1 0
```

```r
#> [1] 1 0"

# My Question: what is the difference of 
df[1]
```

```
## # A tibble: 10 × 1
##               a
##           <dbl>
## 1  -0.161319335
## 2   1.577140103
## 3  -0.558105492
## 4   0.967952411
## 5  -1.169620998
## 6   0.009886324
## 7   0.890840332
## 8   0.294667490
## 9  -0.029859358
## 10  2.250867592
```

```r
df[[1]]
```

```
##  [1] -0.161319335  1.577140103 -0.558105492  0.967952411 -1.169620998
##  [6]  0.009886324  0.890840332  0.294667490 -0.029859358  2.250867592
```

```r
df[,1]
```

```
## # A tibble: 10 × 1
##               a
##           <dbl>
## 1  -0.161319335
## 2   1.577140103
## 3  -0.558105492
## 4   0.967952411
## 5  -1.169620998
## 6   0.009886324
## 7   0.890840332
## 8   0.294667490
## 9  -0.029859358
## 10  2.250867592
```

```r
# why double blackets? in the body above? df[[i]] and output[[i]]
```
# 21.2.1 Exercises

```r
#1. Write for loops to:
## Compute the mean of every column in mtcars.
mtcars
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

```r
output.mtcars.mean<-vector()
for(i in seq_along(mtcars)) {
  output.mtcars.mean[i]<-mean(mtcars[,i])
}
output.mtcars.mean
```

```
##  [1]  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250
##  [7]  17.848750   0.437500   0.406250   3.687500   2.812500
```

```r
typeof(output.mtcars.mean)
```

```
## [1] "double"
```

```r
# 2nd version
output.mtcars.mean<-vector("double",ncol(mtcars)) # why do I need to specify?
for(i in seq_along(mtcars)) {
  output.mtcars.mean[i]<-mean(mtcars[,i])
}
output.mtcars.mean
```

```
##  [1]  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250
##  [7]  17.848750   0.437500   0.406250   3.687500   2.812500
```

```r
typeof(output.mtcars.mean)
```

```
## [1] "double"
```

```r
# v3
output.mtcars.mean<-vector("double",ncol(mtcars)) # why do I need to specify?
for(i in seq_along(mtcars)) {
  output.mtcars.mean[[i]]<-mean(mtcars[,i])
}
output.mtcars.mean
```

```
##  [1]  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250
##  [7]  17.848750   0.437500   0.406250   3.687500   2.812500
```

```r
typeof(output.mtcars.mean)
```

```
## [1] "double"
```

```r
## Determine the type of each column in nycflights13::flights.
nycflights13::flights
```

```
## # A tibble: 336,776 × 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>
## 1   2013     1     1      517            515         2      830
## 2   2013     1     1      533            529         4      850
## 3   2013     1     1      542            540         2      923
## 4   2013     1     1      544            545        -1     1004
## 5   2013     1     1      554            600        -6      812
## 6   2013     1     1      554            558        -4      740
## 7   2013     1     1      555            600        -5      913
## 8   2013     1     1      557            600        -3      709
## 9   2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # ... with 336,766 more rows, and 12 more variables: sched_arr_time <int>,
## #   arr_delay <dbl>, carrier <chr>, flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

```r
type.flights.col<-vector()
for(i in seq_along(nycflights13::flights)) {
  type.flights.col[i]<-typeof(nycflights13::flights[[i]])
}
type.flights.col
```

```
##  [1] "integer"   "integer"   "integer"   "integer"   "integer"  
##  [6] "double"    "integer"   "integer"   "double"    "character"
## [11] "integer"   "character" "character" "character" "double"   
## [16] "double"    "double"    "double"    "double"
```

```r
str(nycflights13::flights) # this is different from type.flights.col at the last time_hour column
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	336776 obs. of  19 variables:
##  $ year          : int  2013 2013 2013 2013 2013 2013 2013 2013 2013 2013 ...
##  $ month         : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ day           : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ dep_time      : int  517 533 542 544 554 554 555 557 557 558 ...
##  $ sched_dep_time: int  515 529 540 545 600 558 600 600 600 600 ...
##  $ dep_delay     : num  2 4 2 -1 -6 -4 -5 -3 -3 -2 ...
##  $ arr_time      : int  830 850 923 1004 812 740 913 709 838 753 ...
##  $ sched_arr_time: int  819 830 850 1022 837 728 854 723 846 745 ...
##  $ arr_delay     : num  11 20 33 -18 -25 12 19 -14 -8 8 ...
##  $ carrier       : chr  "UA" "UA" "AA" "B6" ...
##  $ flight        : int  1545 1714 1141 725 461 1696 507 5708 79 301 ...
##  $ tailnum       : chr  "N14228" "N24211" "N619AA" "N804JB" ...
##  $ origin        : chr  "EWR" "LGA" "JFK" "JFK" ...
##  $ dest          : chr  "IAH" "IAH" "MIA" "BQN" ...
##  $ air_time      : num  227 227 160 183 116 150 158 53 140 138 ...
##  $ distance      : num  1400 1416 1089 1576 762 ...
##  $ hour          : num  5 5 5 5 6 5 6 6 6 6 ...
##  $ minute        : num  15 29 40 45 0 58 0 0 0 0 ...
##  $ time_hour     : POSIXct, format: "2013-01-01 05:00:00" "2013-01-01 05:00:00" ...
```

```r
## Compute the number of unique values in each column of iris.
iris
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
## 1            5.1         3.5          1.4         0.2     setosa
## 2            4.9         3.0          1.4         0.2     setosa
## 3            4.7         3.2          1.3         0.2     setosa
## 4            4.6         3.1          1.5         0.2     setosa
## 5            5.0         3.6          1.4         0.2     setosa
## 6            5.4         3.9          1.7         0.4     setosa
## 7            4.6         3.4          1.4         0.3     setosa
## 8            5.0         3.4          1.5         0.2     setosa
## 9            4.4         2.9          1.4         0.2     setosa
## 10           4.9         3.1          1.5         0.1     setosa
## 11           5.4         3.7          1.5         0.2     setosa
## 12           4.8         3.4          1.6         0.2     setosa
## 13           4.8         3.0          1.4         0.1     setosa
## 14           4.3         3.0          1.1         0.1     setosa
## 15           5.8         4.0          1.2         0.2     setosa
## 16           5.7         4.4          1.5         0.4     setosa
## 17           5.4         3.9          1.3         0.4     setosa
## 18           5.1         3.5          1.4         0.3     setosa
## 19           5.7         3.8          1.7         0.3     setosa
## 20           5.1         3.8          1.5         0.3     setosa
## 21           5.4         3.4          1.7         0.2     setosa
## 22           5.1         3.7          1.5         0.4     setosa
## 23           4.6         3.6          1.0         0.2     setosa
## 24           5.1         3.3          1.7         0.5     setosa
## 25           4.8         3.4          1.9         0.2     setosa
## 26           5.0         3.0          1.6         0.2     setosa
## 27           5.0         3.4          1.6         0.4     setosa
## 28           5.2         3.5          1.5         0.2     setosa
## 29           5.2         3.4          1.4         0.2     setosa
## 30           4.7         3.2          1.6         0.2     setosa
## 31           4.8         3.1          1.6         0.2     setosa
## 32           5.4         3.4          1.5         0.4     setosa
## 33           5.2         4.1          1.5         0.1     setosa
## 34           5.5         4.2          1.4         0.2     setosa
## 35           4.9         3.1          1.5         0.2     setosa
## 36           5.0         3.2          1.2         0.2     setosa
## 37           5.5         3.5          1.3         0.2     setosa
## 38           4.9         3.6          1.4         0.1     setosa
## 39           4.4         3.0          1.3         0.2     setosa
## 40           5.1         3.4          1.5         0.2     setosa
## 41           5.0         3.5          1.3         0.3     setosa
## 42           4.5         2.3          1.3         0.3     setosa
## 43           4.4         3.2          1.3         0.2     setosa
## 44           5.0         3.5          1.6         0.6     setosa
## 45           5.1         3.8          1.9         0.4     setosa
## 46           4.8         3.0          1.4         0.3     setosa
## 47           5.1         3.8          1.6         0.2     setosa
## 48           4.6         3.2          1.4         0.2     setosa
## 49           5.3         3.7          1.5         0.2     setosa
## 50           5.0         3.3          1.4         0.2     setosa
## 51           7.0         3.2          4.7         1.4 versicolor
## 52           6.4         3.2          4.5         1.5 versicolor
## 53           6.9         3.1          4.9         1.5 versicolor
## 54           5.5         2.3          4.0         1.3 versicolor
## 55           6.5         2.8          4.6         1.5 versicolor
## 56           5.7         2.8          4.5         1.3 versicolor
## 57           6.3         3.3          4.7         1.6 versicolor
## 58           4.9         2.4          3.3         1.0 versicolor
## 59           6.6         2.9          4.6         1.3 versicolor
## 60           5.2         2.7          3.9         1.4 versicolor
## 61           5.0         2.0          3.5         1.0 versicolor
## 62           5.9         3.0          4.2         1.5 versicolor
## 63           6.0         2.2          4.0         1.0 versicolor
## 64           6.1         2.9          4.7         1.4 versicolor
## 65           5.6         2.9          3.6         1.3 versicolor
## 66           6.7         3.1          4.4         1.4 versicolor
## 67           5.6         3.0          4.5         1.5 versicolor
## 68           5.8         2.7          4.1         1.0 versicolor
## 69           6.2         2.2          4.5         1.5 versicolor
## 70           5.6         2.5          3.9         1.1 versicolor
## 71           5.9         3.2          4.8         1.8 versicolor
## 72           6.1         2.8          4.0         1.3 versicolor
## 73           6.3         2.5          4.9         1.5 versicolor
## 74           6.1         2.8          4.7         1.2 versicolor
## 75           6.4         2.9          4.3         1.3 versicolor
## 76           6.6         3.0          4.4         1.4 versicolor
## 77           6.8         2.8          4.8         1.4 versicolor
## 78           6.7         3.0          5.0         1.7 versicolor
## 79           6.0         2.9          4.5         1.5 versicolor
## 80           5.7         2.6          3.5         1.0 versicolor
## 81           5.5         2.4          3.8         1.1 versicolor
## 82           5.5         2.4          3.7         1.0 versicolor
## 83           5.8         2.7          3.9         1.2 versicolor
## 84           6.0         2.7          5.1         1.6 versicolor
## 85           5.4         3.0          4.5         1.5 versicolor
## 86           6.0         3.4          4.5         1.6 versicolor
## 87           6.7         3.1          4.7         1.5 versicolor
## 88           6.3         2.3          4.4         1.3 versicolor
## 89           5.6         3.0          4.1         1.3 versicolor
## 90           5.5         2.5          4.0         1.3 versicolor
## 91           5.5         2.6          4.4         1.2 versicolor
## 92           6.1         3.0          4.6         1.4 versicolor
## 93           5.8         2.6          4.0         1.2 versicolor
## 94           5.0         2.3          3.3         1.0 versicolor
## 95           5.6         2.7          4.2         1.3 versicolor
## 96           5.7         3.0          4.2         1.2 versicolor
## 97           5.7         2.9          4.2         1.3 versicolor
## 98           6.2         2.9          4.3         1.3 versicolor
## 99           5.1         2.5          3.0         1.1 versicolor
## 100          5.7         2.8          4.1         1.3 versicolor
## 101          6.3         3.3          6.0         2.5  virginica
## 102          5.8         2.7          5.1         1.9  virginica
## 103          7.1         3.0          5.9         2.1  virginica
## 104          6.3         2.9          5.6         1.8  virginica
## 105          6.5         3.0          5.8         2.2  virginica
## 106          7.6         3.0          6.6         2.1  virginica
## 107          4.9         2.5          4.5         1.7  virginica
## 108          7.3         2.9          6.3         1.8  virginica
## 109          6.7         2.5          5.8         1.8  virginica
## 110          7.2         3.6          6.1         2.5  virginica
## 111          6.5         3.2          5.1         2.0  virginica
## 112          6.4         2.7          5.3         1.9  virginica
## 113          6.8         3.0          5.5         2.1  virginica
## 114          5.7         2.5          5.0         2.0  virginica
## 115          5.8         2.8          5.1         2.4  virginica
## 116          6.4         3.2          5.3         2.3  virginica
## 117          6.5         3.0          5.5         1.8  virginica
## 118          7.7         3.8          6.7         2.2  virginica
## 119          7.7         2.6          6.9         2.3  virginica
## 120          6.0         2.2          5.0         1.5  virginica
## 121          6.9         3.2          5.7         2.3  virginica
## 122          5.6         2.8          4.9         2.0  virginica
## 123          7.7         2.8          6.7         2.0  virginica
## 124          6.3         2.7          4.9         1.8  virginica
## 125          6.7         3.3          5.7         2.1  virginica
## 126          7.2         3.2          6.0         1.8  virginica
## 127          6.2         2.8          4.8         1.8  virginica
## 128          6.1         3.0          4.9         1.8  virginica
## 129          6.4         2.8          5.6         2.1  virginica
## 130          7.2         3.0          5.8         1.6  virginica
## 131          7.4         2.8          6.1         1.9  virginica
## 132          7.9         3.8          6.4         2.0  virginica
## 133          6.4         2.8          5.6         2.2  virginica
## 134          6.3         2.8          5.1         1.5  virginica
## 135          6.1         2.6          5.6         1.4  virginica
## 136          7.7         3.0          6.1         2.3  virginica
## 137          6.3         3.4          5.6         2.4  virginica
## 138          6.4         3.1          5.5         1.8  virginica
## 139          6.0         3.0          4.8         1.8  virginica
## 140          6.9         3.1          5.4         2.1  virginica
## 141          6.7         3.1          5.6         2.4  virginica
## 142          6.9         3.1          5.1         2.3  virginica
## 143          5.8         2.7          5.1         1.9  virginica
## 144          6.8         3.2          5.9         2.3  virginica
## 145          6.7         3.3          5.7         2.5  virginica
## 146          6.7         3.0          5.2         2.3  virginica
## 147          6.3         2.5          5.0         1.9  virginica
## 148          6.5         3.0          5.2         2.0  virginica
## 149          6.2         3.4          5.4         2.3  virginica
## 150          5.9         3.0          5.1         1.8  virginica
```

```r
num.unique<-vector()
for(i in seq_along(iris)) {
  num.unique[i]<-length(unique(iris[,i]))
}
num.unique
```

```
## [1] 35 23 43 22  3
```

```r
## Generate 10 random normals for each of μ=−10 , 0 , 10 , and 100 .
mu<-c(-10,0,10,100)
normals<-matrix(nrow=10,ncol=length(mu))
colnames(normals)<-mu
for(i in seq_along(mu)) {
normals[,i]<-rnorm(10,mean=mu[i])
}
normals
```

```
##              -10           0        10       100
##  [1,] -10.567134  0.41822802  9.194160  98.00139
##  [2,]  -8.010528  0.31490285  9.380957 100.58462
##  [3,]  -8.220807 -1.47635735  9.236642 101.48790
##  [4,]  -9.168822 -0.87291243 10.000320 100.43444
##  [5,]  -8.190484 -0.06893606  8.964301  99.79582
##  [6,] -10.655496  0.25604670  8.596163  99.55388
##  [7,]  -8.169108 -0.71564092  9.177942 100.71112
##  [8,]  -9.139157 -0.85665476 12.151133  98.73512
##  [9,]  -9.222893 -0.88330339  9.302402  99.61897
## [10,]  -9.335589  0.24336094 11.396526 100.51915
```

```r
# Think about the output, sequence, and body before you start writing the loop.

#2. Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors
## problem 1
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}
## my answer
stringr::str_c(letters,collapse = "")
```

```
## [1] "abcdefghijklmnopqrstuvwxyz"
```

```r
## problem2
x <- sample(100)
sd <- 0
for (i in seq_along(x)) {
  sd <- sd + (x[i] - mean(x)) ^ 2
}
sd <- sqrt(sd / (length(x) - 1))
sd
```

```
## [1] 29.01149
```

```r
## my answer
x
```

```
##   [1]   3  99  60  63  80  89  65  95  55  17  26  87  97  93  56  24  51
##  [18]  79  66   7  33  57  90  47  92  71  37  98  10  94  52  35  78  46
##  [35]   1  27  36  82  25  28   8   9  20  53  49  48  64  15 100  12  38
##  [52]  91  69  13  68  67  84  18  74  81  96  39  77  50  41  45  85  44
##  [69]  58  40  61  14   4   6  70  76  30  75  88  86  72  29  22  43   5
##  [86]  23  31  54  32  83  11  21  42  19  73  62  34  16  59   2
```

```r
sqrt(sum((x - mean(x))^2)/(length(x) -1)) # same as sd
```

```
## [1] 29.01149
```

```r
## problem 3
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}
out
```

```
##   [1]  0.3627966  0.4168788  1.2670260  1.5428877  2.3397761  3.0713466
##   [7]  3.8233485  4.7780722  4.7787544  4.9906950  5.6932558  6.3939828
##  [13]  6.8528791  7.0112474  7.5464515  7.6637541  7.8832861  8.3370689
##  [19]  8.9610582  9.3524788 10.2403890 11.1846384 11.7421797 12.1500189
##  [25] 12.4378787 13.3566899 14.0885981 14.8753247 15.2941678 15.6824807
##  [31] 16.5723493 16.6609903 16.6613676 17.1896573 17.4329386 17.4542526
##  [37] 18.2359111 18.6540086 19.5065363 20.1669497 20.2211569 21.1034742
##  [43] 21.8367497 21.9274239 22.3167612 22.8978327 23.2375086 24.1273016
##  [49] 24.4819181 25.2587653 25.4583882 26.1464408 26.9252124 27.6757999
##  [55] 28.5031804 29.3837255 29.9546093 30.2071922 30.9088707 31.6388759
##  [61] 32.6333604 32.7294751 33.2618188 33.4597630 34.4126804 35.1807683
##  [67] 36.1519571 36.1751486 36.7619673 36.8395216 37.0564478 37.3582279
##  [73] 37.4918516 37.5167768 38.1296984 38.6525538 38.8476559 39.7309044
##  [79] 40.0510975 40.4546497 40.7453839 41.6504691 42.4008881 43.3190725
##  [85] 43.7588890 44.2803004 44.3891975 45.3645107 46.3212061 46.8973739
##  [91] 47.4569547 48.1221100 49.0901949 49.8951866 49.9363081 50.9140407
##  [97] 51.0707463 51.2115954 51.3995063 51.5137238
```

```r
## my answer
identical(cumsum(x) ,out)
```

```
## [1] TRUE
```

```r
#3. Combine your function writing and for loop skills:
##1. Write a for loop that prints() the lyrics to the children’s song “Alice the camel”.
##2. Convert the nursery rhyme “ten in the bed” to a function. Generalise it to any number of people in any sleeping structure.
##3. Convert the nursery rhyme “ten in the bed” to a function. Generalise it to any number of people in any sleeping structure.

#4. It’s common to see for loops that don’t preallocate the output and instead increase the length of a vector at each step:
output <- vector("integer", 0)
for (i in seq_along(x)) {
  output <- c(output, lengths(x[[i]]))
}
output
```

```
##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [36] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
##  [71] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```

```r
## How does this affect performance? Design and execute an experiment.
```
# 21.3 For loop variations

# 21.3.5 Exercises

```r
#1. Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, files <- dir("data/", pattern = "\\.csv$", full.names = TRUE), and now want to read each one with read_csv(). Write the for loop that will load them into a single data frame.
files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)
# if all sheets have the same column numbers
temp<-load(files[1])
```

```
## Warning in readChar(con, 5L, useBytes = TRUE): cannot open compressed file
## 'NA', probable reason 'No such file or directory'
```

```
## Error in readChar(con, 5L, useBytes = TRUE): cannot open the connection
```

```r
for(x in files[-1]) {
  temp2<-load(x)
  temp<-rbind(temp,temp2)
}
temp
```

```
## Error in eval(expr, envir, enclos): object 'temp' not found
```

```r
# if not. store all in list and unlist them?
merged.data<-list()
for(x in files) {
  merged.data[[x]]<-load(x)
}
## convert list into data.frame
df <- enframe(merged.data) # tibble

#2. What happens if you use for (nm in names(x)) and x has no names? What if only some of the elements are named? What if the names are not unique?
# x <- runif(100)
# names(x)
# out <- vector("numeric", length(x))
# out[1] <- x[1]
# for (nm in names(x)[-1]) {
#   out[nm] <- out[nm - 1] + x[i]
# }
# out


#3. Write a function that prints the mean of each numeric column in a data frame, along with its name. For example, show_mean(iris) would print:
show_mean(iris)
```

```
## Error in eval(expr, envir, enclos): could not find function "show_mean"
```

```r
#> Sepal.Length: 5.84
#> Sepal.Width:  3.06
#> Petal.Length: 3.76
#> Petal.Width:  1.20

show_mean<-function(df) {
  for(x in colnames(df)) {
    if(is.numeric(df[[x]])==TRUE) { # only numeric column
      temp<-round(mean(df[[x]]),2)
      cat(paste(x,": ",sep=""))
      cat(temp)
      cat("\n")
  } else {next}
  }
}
show_mean(iris)
```

```
## Sepal.Length: 5.84
## Sepal.Width: 3.06
## Petal.Length: 3.76
## Petal.Width: 1.2
```

```r
# (Extra challenge: what function did I use to make sure that the numbers lined up nicely, even though the variable names had different lengths?)

#4. What does this code do? How does it work?
mtcars.original<-mtcars
trans <- list( 
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}

# answer
#diff(mtcars.original,mtcars)
mtcars.original
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

```r
mtcars
```

```
##                      mpg cyl     disp  hp drat    wt  qsec vs     am gear
## Mazda RX4           21.0   6 2.621936 110 3.90 2.620 16.46  0 manual    4
## Mazda RX4 Wag       21.0   6 2.621936 110 3.90 2.875 17.02  0 manual    4
## Datsun 710          22.8   4 1.769807  93 3.85 2.320 18.61  1 manual    4
## Hornet 4 Drive      21.4   6 4.227872 110 3.08 3.215 19.44  1   auto    3
## Hornet Sportabout   18.7   8 5.899356 175 3.15 3.440 17.02  0   auto    3
## Valiant             18.1   6 3.687098 105 2.76 3.460 20.22  1   auto    3
## Duster 360          14.3   8 5.899356 245 3.21 3.570 15.84  0   auto    3
## Merc 240D           24.4   4 2.403988  62 3.69 3.190 20.00  1   auto    4
## Merc 230            22.8   4 2.307304  95 3.92 3.150 22.90  1   auto    4
## Merc 280            19.2   6 2.746478 123 3.92 3.440 18.30  1   auto    4
## Merc 280C           17.8   6 2.746478 123 3.92 3.440 18.90  1   auto    4
## Merc 450SE          16.4   8 4.519562 180 3.07 4.070 17.40  0   auto    3
## Merc 450SL          17.3   8 4.519562 180 3.07 3.730 17.60  0   auto    3
## Merc 450SLC         15.2   8 4.519562 180 3.07 3.780 18.00  0   auto    3
## Cadillac Fleetwood  10.4   8 7.734711 205 2.93 5.250 17.98  0   auto    3
## Lincoln Continental 10.4   8 7.538066 215 3.00 5.424 17.82  0   auto    3
## Chrysler Imperial   14.7   8 7.210324 230 3.23 5.345 17.42  0   auto    3
## Fiat 128            32.4   4 1.289665  66 4.08 2.200 19.47  1 manual    4
## Honda Civic         30.4   4 1.240503  52 4.93 1.615 18.52  1 manual    4
## Toyota Corolla      33.9   4 1.165123  65 4.22 1.835 19.90  1 manual    4
## Toyota Corona       21.5   4 1.968091  97 3.70 2.465 20.01  1   auto    3
## Dodge Challenger    15.5   8 5.211098 150 2.76 3.520 16.87  0   auto    3
## AMC Javelin         15.2   8 4.981678 150 3.15 3.435 17.30  0   auto    3
## Camaro Z28          13.3   8 5.735485 245 3.73 3.840 15.41  0   auto    3
## Pontiac Firebird    19.2   8 6.554840 175 3.08 3.845 17.05  0   auto    3
## Fiat X1-9           27.3   4 1.294581  66 4.08 1.935 18.90  1 manual    4
## Porsche 914-2       26.0   4 1.971368  91 4.43 2.140 16.70  0 manual    5
## Lotus Europa        30.4   4 1.558413 113 3.77 1.513 16.90  1 manual    5
## Ford Pantera L      15.8   8 5.751872 264 4.22 3.170 14.50  0 manual    5
## Ferrari Dino        19.7   6 2.376130 175 3.62 2.770 15.50  0 manual    5
## Maserati Bora       15.0   8 4.932517 335 3.54 3.570 14.60  0 manual    5
## Volvo 142E          21.4   4 1.982839 109 4.11 2.780 18.60  1 manual    4
##                     carb
## Mazda RX4              4
## Mazda RX4 Wag          4
## Datsun 710             1
## Hornet 4 Drive         1
## Hornet Sportabout      2
## Valiant                1
## Duster 360             4
## Merc 240D              2
## Merc 230               2
## Merc 280               4
## Merc 280C              4
## Merc 450SE             3
## Merc 450SL             3
## Merc 450SLC            3
## Cadillac Fleetwood     4
## Lincoln Continental    4
## Chrysler Imperial      4
## Fiat 128               1
## Honda Civic            2
## Toyota Corolla         1
## Toyota Corona          1
## Dodge Challenger       2
## AMC Javelin            2
## Camaro Z28             4
## Pontiac Firebird       2
## Fiat X1-9              1
## Porsche 914-2          2
## Lotus Europa           2
## Ford Pantera L         4
## Ferrari Dino           6
## Maserati Bora          8
## Volvo 142E             2
```

```r
# transform "disp" column nad "am" column
# two separate functions are used according to two different objects ("disp" or "am"). Input "x" is difined within parenthesis.
```
# 21.4 For loops vs. functionals

```r
df <- tibble(
  a = rnorm(10),
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
```

# 21.4.1 Exercises

```r
#1. Read the documentation for apply(). In the 2d case, what two for loops does it generalise?
?apply

#2. Adapt col_summary() so that it only applies to numeric columns You might want to start with an is_numeric() function that returns a logical vector that has a TRUE corresponding to each numeric column.
col_summary <- function(df, fun) {
  out <- vector("double", length(df))
  for (i in seq_along(df)) {
    out[i] <- fun(df[[i]])
  }
  out
}
col_summary(df, median) # original
```

```
## [1] -0.4223764  0.1797578  0.5726320 -0.3222179
```

```r
# answer
df2 <- tibble(
  a = rnorm(10),
  x = letters[1:10],
  b = rnorm(10),
  c = rnorm(10),
  d = rnorm(10)
)
# 
col_summary2 <- function(y, fun) {
  out <- vector()
    for (i in seq_along(y)) {
      if(is_numeric(y[[i]])==TRUE) {
      out[[i]] <- fun(y[[i]])
      names(out[i]) <- colnames(y)[i] # does not work
      }  else {next}
    }
  out
}
col_summary2(df2,median) # incomplete
```

```
## [1] -0.09513901          NA -0.28887424 -0.17508553  0.40660500
```

# 21.5 The map functions


```r
map_dbl(df, mean)
```

```
##          a          b          c          d 
## -0.1499447  0.1183540  0.5689405 -0.2297504
```

```r
map_dbl(df, median)
```

```
##          a          b          c          d 
## -0.4223764  0.1797578  0.5726320 -0.3222179
```

```r
map_dbl(df, sd)
```

```
## Error: Result 1 is not a length 1 atomic vector
```

```r
df %>% map_dbl(mean)
```

```
##          a          b          c          d 
## -0.1499447  0.1183540  0.5689405 -0.2297504
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
## Warning in mean.default(.x[[i]], ...): argument is not numeric or logical:
## returning NA
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500   3.780862 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500         NA   3.687500   2.812500
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
mu %>% map(function(x) rnorm(10,x))
```

```
## [[1]]
##  [1] -10.104389  -8.745139 -10.292975 -10.190574  -9.634442  -7.469552
##  [7]  -9.872625 -11.733763  -8.761661 -10.620875
## 
## [[2]]
##  [1]  1.42409222 -0.19412662 -0.08817795  1.62613324 -0.96516904
##  [6] -0.30404117  0.03865326  0.81624880  1.40785679  0.80934522
## 
## [[3]]
##  [1] 10.120634 10.297635  8.127723  9.986206  9.056613  9.019030  9.998425
##  [8] 10.851059  9.827334  9.738803
## 
## [[4]]
##  [1]  99.72721 100.37506 100.73889  99.46775 100.82638 100.21300 101.54399
##  [8]  99.24789 100.56947  99.35159
```

```r
#2. How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?
str(df2)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	10 obs. of  5 variables:
##  $ a: num  -0.018 1.418 0.849 -0.172 -0.336 ...
##  $ x: chr  "a" "b" "c" "d" ...
##  $ b: num  0.115 -0.244 -1.347 -0.568 0.89 ...
##  $ c: num  -0.73 -0.467 -0.138 1.32 0.636 ...
##  $ d: num  1.571 0.584 -0.739 -0.89 1.423 ...
```

```r
df3<-df2
df3$x<-as.factor(df3$x)
attributes(str(df3))
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	10 obs. of  5 variables:
##  $ a: num  -0.018 1.418 0.849 -0.172 -0.336 ...
##  $ x: Factor w/ 10 levels "a","b","c","d",..: 1 2 3 4 5 6 7 8 9 10
##  $ b: num  0.115 -0.244 -1.347 -0.568 0.89 ...
##  $ c: num  -0.73 -0.467 -0.138 1.32 0.636 ...
##  $ d: num  1.571 0.584 -0.739 -0.89 1.423 ...
```

```
## NULL
```

```r
#3. What happens when you use the map functions on vectors that aren’t lists? What does map(1:5, runif) do? Why?

#4. What does map(-2:2, rnorm, n = 5) do? Why? What does map_dbl(-2:2, rnorm, n = 5) do? Why?

#5. Rewrite map(x, function(df) lm(mpg ~ wt, data = df)) to eliminate the anonymous function.
```
# 21.6 Dealing with failure

