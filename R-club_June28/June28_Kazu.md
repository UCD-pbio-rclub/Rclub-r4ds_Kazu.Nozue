# June21_Kazu
Kazu  
6/28/2017  


# 13 Relational data

```r
library(nycflights13)
```
# 13.2 nycflights13
# 13.2.1 Exercises

```r
#1. Imagine you wanted to draw (approximately) the route each plane flies from its origin to its destination. What variables would you need? What tables would you need to combine?

#2. I forgot to draw the relationship between weather and airports. What is the relationship and how should it appear in the diagram?
weather
```

```
## # A tibble: 26,130 × 15
##    origin  year month   day  hour  temp  dewp humid wind_dir wind_speed
##     <chr> <dbl> <dbl> <int> <int> <dbl> <dbl> <dbl>    <dbl>      <dbl>
## 1     EWR  2013     1     1     0 37.04 21.92 53.97      230   10.35702
## 2     EWR  2013     1     1     1 37.04 21.92 53.97      230   13.80936
## 3     EWR  2013     1     1     2 37.94 21.92 52.09      230   12.65858
## 4     EWR  2013     1     1     3 37.94 23.00 54.51      230   13.80936
## 5     EWR  2013     1     1     4 37.94 24.08 57.04      240   14.96014
## 6     EWR  2013     1     1     6 39.02 26.06 59.37      270   10.35702
## 7     EWR  2013     1     1     7 39.02 26.96 61.63      250    8.05546
## 8     EWR  2013     1     1     8 39.02 28.04 64.43      240   11.50780
## 9     EWR  2013     1     1     9 39.92 28.04 62.21      250   12.65858
## 10    EWR  2013     1     1    10 39.02 28.04 64.43      260   12.65858
## # ... with 26,120 more rows, and 5 more variables: wind_gust <dbl>,
## #   precip <dbl>, pressure <dbl>, visib <dbl>, time_hour <dttm>
```

```r
#3. weather only contains information for the origin (NYC) airports. If it contained weather records for all airports in the USA, what additional relation would it define with flights?
```
#13.3 Keys
## The variables used to connect each pair of tables are called keys

```r
planes %>% 
  count(tailnum) %>% 
  filter(n > 1)
```

```
## # A tibble: 0 × 2
## # ... with 2 variables: tailnum <chr>, n <int>
```

```r
# 
weather %>% 
  count(year, month, day, hour, origin) %>% 
  filter(n > 1)
```

```
## Source: local data frame [0 x 6]
## Groups: year, month, day, hour [0]
## 
## # ... with 6 variables: year <dbl>, month <dbl>, day <int>, hour <int>,
## #   origin <chr>, n <int>
```

```r
#
flights %>% 
  count(year, month, day, flight) %>% 
  filter(n > 1)
```

```
## Source: local data frame [29,768 x 5]
## Groups: year, month, day [365]
## 
##     year month   day flight     n
##    <int> <int> <int>  <int> <int>
## 1   2013     1     1      1     2
## 2   2013     1     1      3     2
## 3   2013     1     1      4     2
## 4   2013     1     1     11     3
## 5   2013     1     1     15     2
## 6   2013     1     1     21     2
## 7   2013     1     1     27     4
## 8   2013     1     1     31     2
## 9   2013     1     1     32     2
## 10  2013     1     1     35     2
## # ... with 29,758 more rows
```

```r
#
flights %>% 
  count(year, month, day, tailnum) %>% 
  filter(n > 1)
```

```
## Source: local data frame [64,928 x 5]
## Groups: year, month, day [365]
## 
##     year month   day tailnum     n
##    <int> <int> <int>   <chr> <int>
## 1   2013     1     1  N0EGMQ     2
## 2   2013     1     1  N11189     2
## 3   2013     1     1  N11536     2
## 4   2013     1     1  N11544     3
## 5   2013     1     1  N11551     2
## 6   2013     1     1  N12540     2
## 7   2013     1     1  N12567     2
## 8   2013     1     1  N13123     2
## 9   2013     1     1  N13538     3
## 10  2013     1     1  N13566     3
## # ... with 64,918 more rows
```

```r
# When starting to work with this data, I had naively assumed that each flight number would be only used once per day: that would make it much easier to communicate problems with a specific flight. Unfortunately that is not the case! If a table lacks a primary key, it’s sometimes useful to add one with mutate() and row_number(). That makes it easier to match observations if you’ve done some filtering and want to check back in with the original data. This is called a surrogate key.
?row_number()
# A primary key and the corresponding foreign key in another table form a relation. Relations are typically one-to-many. For example, each flight has one plane, but each plane has many flights. In other data, you’ll occasionally see a 1-to-1 relationship. You can think of this as a special case of 1-to-many. You can model many-to-many relations with a many-to-1 relation plus a 1-to-many relation. For example, in this data there’s a many-to-many relationship between airlines and airports: each airline flies to many airports; each airport hosts many airlines.
```
#13.3.1 Exercises

```r
#1. Add a surrogate key to flights.
mutate(flights,row_num=row_number()) %>% select(row_num)
```

```
## # A tibble: 336,776 × 1
##    row_num
##      <int>
## 1        1
## 2        2
## 3        3
## 4        4
## 5        5
## 6        6
## 7        7
## 8        8
## 9        9
## 10      10
## # ... with 336,766 more rows
```

```r
#2. Identify the keys in the following datasets
# Lahman::Batting
library(Lahman)
Lahman::Batting %>% colnames() -> Batteing.colname
Lahman::Master %>% colnames() -> Master.colname
Lahman::Pitching %>% colnames() -> Pitching.colname
Lahman::Fielding %>% colnames() -> Fielding.colname
all.colnames <- unique(c(Batteing.colname,Master.colname,Pitching.colname,Fielding.colname))
Lahman.data <- tibble(
  Battering=all.colnames %in%  Batteing.colname,
  Master=all.colnames %in%  Master.colname,
  Pitching=all.colnames %in%  Pitching.colname,
  Fielding=all.colnames %in%  Fielding.colname
)
summary(Lahman.data) # number of TRUE is numbe of keys in each data
```

```
##  Battering         Master         Pitching        Fielding      
##  Mode :logical   Mode :logical   Mode :logical   Mode :logical  
##  FALSE:47        FALSE:43        FALSE:39        FALSE:51       
##  TRUE :22        TRUE :26        TRUE :30        TRUE :18       
##  NA's :0         NA's :0         NA's :0         NA's :0
```

```r
## babynames::babynames

## nasaweather::atmos
## fueleconomy::vehicles
## ggplot2::diamonds
## (You might need to install some packages and read some documentation.)

#3. Draw a diagram illustrating the connections between the Batting, Master, and Salaries tables in the Lahman package. Draw another diagram that shows the relationship between Master, Managers, AwardsManagers.
## How would you characterise the relationship between the Batting, Pitching, and Fielding tables?
```
