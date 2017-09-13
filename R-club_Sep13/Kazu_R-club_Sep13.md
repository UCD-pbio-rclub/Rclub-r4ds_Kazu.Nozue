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
## [1] -0.42578006 -0.09987019 -0.56020767  0.61725058
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
##             a
##         <dbl>
## 1  -0.9981031
## 2  -0.5612054
## 3  -0.2058156
## 4  -0.8175146
## 5  -0.8843192
## 6  -0.1211837
## 7   0.2343327
## 8  -0.2903547
## 9  -1.5583831
## 10  1.0605407
```

```r
df[[1]]
```

```
##  [1] -0.9981031 -0.5612054 -0.2058156 -0.8175146 -0.8843192 -0.1211837
##  [7]  0.2343327 -0.2903547 -1.5583831  1.0605407
```

```r
df[,1]
```

```
## # A tibble: 10 × 1
##             a
##         <dbl>
## 1  -0.9981031
## 2  -0.5612054
## 3  -0.2058156
## 4  -0.8175146
## 5  -0.8843192
## 6  -0.1211837
## 7   0.2343327
## 8  -0.2903547
## 9  -1.5583831
## 10  1.0605407
```

```r
# why double blackets? in the body above? df[[i]] and output[[i]]
# J's answer
x1<-list(c(1,2),c(3,4))
x1[1]
```

```
## [[1]]
## [1] 1 2
```

```r
x1[[1]]
```

```
## [1] 1 2
```

```r
typeof(x1[1]) # list
```

```
## [1] "list"
```

```r
typeof(x1[[1]]) # double
```

```
## [1] "double"
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
# to calculate more efficiently (J)
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
##  [1,] -12.112177 -1.25793153 10.586749  99.20732
##  [2,]  -9.864666 -0.24600398 10.202329 101.47637
##  [3,]  -9.378215  1.17726452 10.331434  99.63414
##  [4,] -11.359508  0.84269786 11.152601  98.74512
##  [5,]  -8.813454 -0.07412346  9.249086 102.41520
##  [6,] -11.846753  1.03128343  9.392074  99.23161
##  [7,]  -9.247854  1.93963997  9.488683 101.01472
##  [8,]  -9.097285  1.42917477 10.927687  97.51010
##  [9,] -10.671218 -0.21770339  6.450667  98.82334
## [10,] -10.154517  1.19542115  8.111334 100.84252
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
##   [1]  39  43  30   2  91  97  74  53  16  44  60  70  12  52  27  38  93
##  [18]  83  92  41  80  72  77  19   7  10  87  31  63  86  26  62   1  35
##  [35]  17  82  20  81  45  79  65  59  61  58  36  42  89  24   6  47  18
##  [52]  90  11  64  51  66  37  94  99  67  33  15   4  50  40  21  75  48
##  [69]  95  13  55  29   9   3  76  88  96  34  32   8  25  98  57  56  69
##  [86]   5  54  68  85  23 100  28  84  71  73  49  22  14  78  46
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
##   [1]  0.04677231  0.85130666  0.92826815  1.72106558  2.71508585
##   [6]  3.34534344  3.55391655  4.47606085  4.76984241  4.85189653
##  [11]  4.99206766  5.33305075  5.97416330  6.55909012  7.55660060
##  [16]  7.83453886  8.60148027  8.90631128  9.27617038 10.19827445
##  [21] 10.66265256 11.16888131 11.43558434 11.87141437 12.76032927
##  [26] 13.08672305 13.59619714 13.61797830 14.52885726 15.25252605
##  [31] 15.43770619 15.60126778 16.14062620 16.88469460 17.63424621
##  [36] 18.61660471 19.56080219 20.31441352 20.85894971 21.82197024
##  [41] 22.68854350 22.87979319 23.59183250 24.47695079 25.15130319
##  [46] 25.80414732 26.14127962 26.64981930 27.37413154 28.21460692
##  [51] 29.01355316 29.62959129 29.93507742 30.77335360 31.67317441
##  [56] 32.07674134 32.43595820 32.86623681 33.14541448 33.19604998
##  [61] 33.44103087 34.18536805 34.54723452 35.29824800 35.63501868
##  [66] 36.57561053 36.93023962 37.78252673 38.27234806 39.09628106
##  [71] 39.90409097 40.67717451 40.78562592 41.23962312 42.12645856
##  [76] 42.90387507 43.71222822 44.08025603 44.40551492 44.79597838
##  [81] 44.92710440 45.27706625 45.33393832 45.54326752 46.06477730
##  [86] 46.71278939 47.50954488 48.29791035 49.23116948 49.62771199
##  [91] 50.37283197 50.92339746 51.30101677 51.39353796 52.15866527
##  [96] 52.75982662 53.40295853 54.25736983 55.09405055 55.54639551
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
# I skipped. See Julin or Stacey's answers
```
# 21.3 For loop variations

# 21.3.5 Exercises

```r
#1. Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, files <- dir("data/", pattern = "\\.csv$", full.names = TRUE), and now want to read each one with read_csv(). Write the for loop that will load them into a single data frame.
files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)
# if all sheets have the same column numbers
temp<-read_csv(files[1])
```

```
## Error: 'NA' does not exist in current working directory ('/Volumes/data_work/Data6/bioconductor_R/Rclub2017_spring/Rclub-r4ds_Kazu.Nozue/R-club_Sep13').
```

```r
for(x in files[-1]) {
  temp2<-read_csv(x)
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
  merged.data[[x]]<-read_csv(x)
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
## [1]  0.29469587 -0.30515987 -0.09446403 -0.19680245
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
      } # else {next}
    }
  out
}
col_summary2(df2,median) # incomplete
```

```
## [1] -1.21862832          NA -0.30132813 -0.20143108 -0.08818903
```

```r
# 
```

# 21.5 The map functions


```r
map_dbl(df, mean)
```

```
##          a          b          c          d 
##  0.2330449 -0.2584854 -0.3265362 -0.2527256
```

```r
map_dbl(df, median)
```

```
##           a           b           c           d 
##  0.29469587 -0.30515987 -0.09446403 -0.19680245
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
##  0.2330449 -0.2584854 -0.3265362 -0.2527256
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
##  [1]  -9.173699  -9.245598 -11.759037 -11.168114  -9.563800  -9.585204
##  [7] -11.762264 -11.289536  -9.680638  -8.361946
## 
## [[2]]
##  [1] -0.74017463 -1.00270676 -2.10793398  0.02839868 -0.65739727
##  [6] -0.34633866  1.74812986  0.20602065  0.46639840  0.18369170
## 
## [[3]]
##  [1] 10.736206  8.655485  9.901215 10.626508  9.354677 11.123646  9.521537
##  [8]  9.789973  9.673017  9.374131
## 
## [[4]]
##  [1] 100.01767  99.51263 101.59060  99.77219  99.29118 100.39991  99.79205
##  [8]  99.20697 100.81831 100.38324
```

```r
mu %>% map(function(x) rnorm(10,mean=x))
```

```
## [[1]]
##  [1]  -8.962919  -9.912537  -9.327342  -9.266466 -10.996637  -9.918477
##  [7]  -9.312038  -9.904522  -8.739972  -8.644404
## 
## [[2]]
##  [1]  0.36038853 -0.74672377 -0.31204561  0.42011754 -0.77720438
##  [6] -0.08335023  1.63401075  0.96179579 -0.48684595 -2.56871269
## 
## [[3]]
##  [1] 11.131665  8.872097  9.765596 10.604743  8.243408 10.430302  9.377313
##  [8]  9.742727  9.826660 11.044122
## 
## [[4]]
##  [1] 100.36871  98.56949  99.15670 100.49726  99.52741 100.65569  99.26634
##  [8]  98.82872  99.76907 101.17410
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
##  $ a: num  0.941 -0.152 -1.282 1.113 0.687 ...
##  $ x: chr  "a" "b" "c" "d" ...
##  $ b: num  -0.395 -0.756 -0.76 0.535 -0.582 ...
##  $ c: num  1.631 0.492 0.381 1.203 0.775 ...
##  $ d: num  -1.106 -1.901 -1.131 0.673 0.748 ...
```

```r
df3<-df2
df3$x<-as.factor(df3$x)
attributes(str(df3))
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	10 obs. of  5 variables:
##  $ a: num  0.941 -0.152 -1.282 1.113 0.687 ...
##  $ x: Factor w/ 10 levels "a","b","c","d",..: 1 2 3 4 5 6 7 8 9 10
##  $ b: num  -0.395 -0.756 -0.76 0.535 -0.582 ...
##  $ c: num  1.631 0.492 0.381 1.203 0.775 ...
##  $ d: num  -1.106 -1.901 -1.131 0.673 0.748 ...
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
## [1] 0.0523393
## 
## [[2]]
## [1] 0.6807549 0.8905001
## 
## [[3]]
## [1] 0.09812893 0.59356093 0.28201578
## 
## [[4]]
## [1] 0.4836408 0.1568726 0.2658584 0.8377906
## 
## [[5]]
## [1] 0.9342574 0.8417619 0.8124335 0.8649559 0.6100002
```

```r
map(1, runif)
```

```
## [[1]]
## [1] 0.005154351
```

```r
map(2, runif)
```

```
## [[1]]
## [1] 0.6311396 0.1837135
```

```r
map(5, runif) 
```

```
## [[1]]
## [1] 0.4345283 0.8079047 0.7219501 0.9140047 0.4534498
```

```r
#4. What does map(-2:2, rnorm, n = 5) do? Why? What does map_dbl(-2:2, rnorm, n = 5) do? Why?
map(-2:2, rnorm, n = 5) # mean is now variable. n is fixed to "5"
```

```
## [[1]]
## [1] -2.818321 -1.217676 -1.282322 -2.536644 -2.769359
## 
## [[2]]
## [1] -1.4462360 -0.1136957 -2.6357911 -0.4935999 -1.5849765
## 
## [[3]]
## [1]  0.4156517 -1.6665530 -0.5334248 -1.0945712  1.0959866
## 
## [[4]]
## [1]  1.1216777 -0.2905201 -0.1556159  0.2459891 -0.1674828
## 
## [[5]]
## [1] -0.1892445  3.6599044  1.2902384  2.6929492  2.8078720
```

```r
#5. Rewrite map(x, function(df) lm(mpg ~ wt, data = df)) to eliminate the anonymous function.
mtcars %>% map(function(df) lm(mpg ~ wt, data = df)) # original, Why this does not work?
```

```
## Error in eval(predvars, data, env): numeric 'envir' arg not of length one
```

```r
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
  split(.$cyl) %>% 
  map(~lm(mpg ~ wt, data = .)) # works..
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
y$result[is_ok] %>% flatten_dbl()
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
##  $ : num [1:5] 4.88 5.36 5.69 4.96 6.05
##  $ : num [1:5] 9.43 9.16 10.62 9.4 8.9
##  $ : num [1:5] -4.23 -2.66 -2.36 -1.14 -1.63
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
##  $ : num [1:5] 3.83 3.84 3.78 4.1 4.88
##  $ : num [1:5] 9.21 3.09 2.52 -5.72 12.32
##  $ : num [1:5] -1.06 -5.17 -12.32 13.93 -21.3
```

```r
#
map2(mu, sigma, rnorm, n = 5) %>% str()
```

```
## List of 3
##  $ : num [1:5] 5.17 5.11 5.47 5.77 5.64
##  $ : num [1:5] 2.43 17.94 13.92 6.35 11.33
##  $ : num [1:5] -6.2 -15.22 -13.89 7.57 7.7
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
#
n <- list(1, 3, 5)
args1 <- list(n, mu, sigma)
args1 %>%
  pmap(rnorm) %>% 
  str()
```

```
## List of 3
##  $ : num 5.18
##  $ : num [1:3] 1.94 7.29 7.33
##  $ : num [1:5] 10.61 4.08 8.5 -12.23 3.12
```

```r
#
args2 <- list(mean = mu, sd = sigma, n = n)
args2 %>% 
  pmap(rnorm) %>% 
  str()
```

```
## List of 3
##  $ : num 4.65
##  $ : num [1:3] 9.93 15.03 13.03
##  $ : num [1:5] 7.98 -9.85 -18.48 5.11 1.1
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
## [1] 3.787039
## 
## [[2]]
## [1] 11.096439  4.713716 17.800128
## 
## [[3]]
## [1]  -8.377811  -4.211184  17.318502 -15.319075  -5.261924
```
# 21.7.1 Invoking different functions

```r
f <- c("runif", "rnorm", "rpois")
param <- list(
  list(min = -1, max = 1), 
  list(sd = 5), 
  list(lambda = 10)
)
```
