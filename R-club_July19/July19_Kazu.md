# Kazu_July12and19
Kazu  
7/18/2017  


# 14 Strings
# 14.1 Introduction
# 14.1.1 Prerequisites

```r
library(tidyverse)
```

```
## Loading tidyverse: ggplot2
## Loading tidyverse: tibble
## Loading tidyverse: tidyr
## Loading tidyverse: readr
## Loading tidyverse: purrr
## Loading tidyverse: dplyr
```

```
## Conflicts with tidy packages ----------------------------------------------
```

```
## filter(): dplyr, stats
## lag():    dplyr, stats
```

```r
library(stringr)
```
# 14.2 String basics

```r
string1 <- "This is a string"
string2 <- 'If I want to include a "quote" inside a string, I use single quotes'
# To include a literal single or double quote in a string you can use \ to “escape” it:
double_quote <- "\"" # or '"'
double_quote
```

```
## [1] "\""
```

```r
single_quote <- '\'' # or "'"
single_quote
```

```
## [1] "'"
```

```r
x <- c("\"", "\\")
x
```

```
## [1] "\"" "\\"
```

```r
#> [1] "\"" "\\"
writeLines(x)
```

```
## "
## \
```

```r
#> "
#> \
# There are a handful of other special characters. The most common are "\n", newline, and "\t", tab, but you can see the complete list by requesting help on ": ?'"', or ?"'". You’ll also sometimes see strings like "\u00b5", this is a way of writing non-English characters that works on all platforms:
?'"' # complete list
x <- "\u00b5"
x
```

```
## [1] "µ"
```

```r
# Multiple strings are often stored in a character vector, which you can create with c():

c("one", "two", "three")
```

```
## [1] "one"   "two"   "three"
```
# 14.2.1 String length

```r
str_length(c("a", "R for data science", NA))
```

```
## [1]  1 18 NA
```
# 14.2.2 Combining strings

```r
# To combine two or more strings, use str_c()
str_c("x", "y") # alternative of paste()?
```

```
## [1] "xy"
```

```r
str_c("x", "y", "z")
```

```
## [1] "xyz"
```

```r
# Use the sep argument to control how they’re separated
str_c("x", "y", sep = ", ")
```

```
## [1] "x, y"
```

```r
# Like most other functions in R, missing values are contagious. If you want them to print as "NA", use str_replace_na()
x <- c("abc", NA)
str_c("|-", x, "-|")
```

```
## [1] "|-abc-|" NA
```

```r
str_c("|-", str_replace_na(x), "-|")
```

```
## [1] "|-abc-|" "|-NA-|"
```

```r
# As shown above, str_c() is vectorised, and it automatically recycles shorter vectors to the same length as the longest
str_c("prefix-", c("a", "b", "c"), "-suffix")
```

```
## [1] "prefix-a-suffix" "prefix-b-suffix" "prefix-c-suffix"
```

```r
# Objects of length 0 are silently dropped. This is particularly useful in conjunction with if
name <- "Hadley"
time_of_day <- "morning"
birthday <- FALSE

str_c(
  "Good ", time_of_day, " ", name,
  if (birthday) " and HAPPY BIRTHDAY",
  "."
)
```

```
## [1] "Good morning Hadley."
```

```r
# To collapse a vector of strings into a single string, use collapse:
str_c(c("x", "y", "z"), collapse = ", ")
```

```
## [1] "x, y, z"
```
# 14.2.3 Subsetting strings

```r
# You can extract parts of a string using str_sub(). As well as the string, str_sub() takes start and end arguments which give the (inclusive) position of the substring
x <- c("Apple", "Banana", "Pear")
str_sub(x, 1, 3)
```

```
## [1] "App" "Ban" "Pea"
```

```r
# negative numbers count backwards from end
str_sub(x, -3, -1)
```

```
## [1] "ple" "ana" "ear"
```

```r
# Note that str_sub() won’t fail if the string is too short: it will just return as much as possible
str_sub("a", 1, 5)
```

```
## [1] "a"
```

```r
# You can also use the assignment form of str_sub() to modify strings
str_sub(x, 1, 1) <- str_to_lower(str_sub(x, 1, 1))
x
```

```
## [1] "apple"  "banana" "pear"
```
# 14.2.4 Locales

```r
# Above I used str_to_lower() to change the text to lower case. You can also use str_to_upper() or str_to_title(). However, changing case is more complicated than it might at first appear because different languages have different rules for changing case. You can pick which set of rules to use by specifying a locale:

# Turkish has two i's: with and without a dot, and it
# has a different rule for capitalising them:
str_to_upper(c("i", "ı"))
```

```
## [1] "I" "I"
```

```r
str_to_upper(c("i", "ı"), locale = "tr") # Turkish version
```

```
## [1] "İ" "I"
```

```r
# The locale is specified as a ISO 639 language code, which is a two or three letter abbreviation. If you don’t already know the code for your language, Wikipedia has a good list. If you leave the locale blank, it will use the current locale, as provided by your operating system.
# Another important operation that’s affected by the locale is sorting. The base R order() and sort() functions sort strings using the current locale. If you want robust behaviour across different computers, you may want to use str_sort() and str_order() which take an additional locale argument:
x <- c("apple", "eggplant", "banana")

str_sort(x, locale = "en")  # English 
```

```
## [1] "apple"    "banana"   "eggplant"
```

```r
str_sort(x, locale = "haw") # Hawaiian
```

```
## [1] "apple"    "eggplant" "banana"
```
# 14.2.5 Exercises

```r
#1 In code that doesn’t use stringr, you’ll often see paste() and paste0(). What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?

#2. In your own words, describe the difference between the sep and collapse arguments to str_c().

#3. Use str_length() and str_sub() to extract the middle character from a string. What will you do if the string has an even number of characters?


#4. What does str_wrap() do? When might you want to use it?

#5. What does str_trim() do? What’s the opposite of str_trim()?

#6. Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.
```
# 14.3 Matching patterns with regular expressions
## 14.3.1 Basic matches

```r
x <- c("apple", "banana", "pear")
str_view(x, "an") # (KN) interesting function!
```

<!--html_preserve--><div id="htmlwidget-7559ab30dd0f10c7b4e9" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-7559ab30dd0f10c7b4e9">{"x":{"html":"<ul>\n  <li>apple<\/li>\n  <li>b<span class='match'>an<\/span>ana<\/li>\n  <li>pear<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# The next step up in complexity is ., which matches any character (except a newline):
str_view(x, ".a.")
```

<!--html_preserve--><div id="htmlwidget-e8b8cfa01872a6d96926" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-e8b8cfa01872a6d96926">{"x":{"html":"<ul>\n  <li>apple<\/li>\n  <li><span class='match'>ban<\/span>ana<\/li>\n  <li>p<span class='match'>ear<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# 
# To create the regular expression, we need \\
dot <- "\\."

# But the expression itself only contains one:
writeLines(dot)
```

```
## \.
```

```r
# And this tells R to look for an explicit .
str_view(c("abc", "a.c", "bef"), "a\\.c")
```

<!--html_preserve--><div id="htmlwidget-b1c0f28eaa6c018787e9" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-b1c0f28eaa6c018787e9">{"x":{"html":"<ul>\n  <li>abc<\/li>\n  <li><span class='match'>a.c<\/span><\/li>\n  <li>bef<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# str_view(c("abc", "a.c", "bef"), "a\.c") # Error: '\.' is an unrecognized escape in character string starting ""a\." ... "Unfortunately this creates a problem. We use strings to represent regular expressions, and \ is also used as an escape symbol in strings." (from book)
# str_view(c("abc", "a.c", "bef"), "a.c") # "abc" and "a.c"

# If \ is used as an escape character in regular expressions, how do you match a literal \? Well you need to escape it, creating the regular expression \\. To create that regular expression, you need to use a string, which also needs to escape \. That means to match a literal \ you need to write "\\\\" — you need four backslashes to match one!

x <- "a\\b"
writeLines(x)
```

```
## a\b
```

```r
str_view(x, "\\\\")
```

<!--html_preserve--><div id="htmlwidget-44e59d0afcd09e29ad1b" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-44e59d0afcd09e29ad1b">{"x":{"html":"<ul>\n  <li>a<span class='match'>\\<\/span>b<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# In this book, I’ll write regular expression as \. and strings that represent the regular expression as "\\.".
```
# 14.3.1.1 Exercises

```r
#1. Explain why each of these strings don’t match a \: "\", "\\", "\\\".
x <- "a\\b"
writeLines(x)
str_view(x, "\") 
# escaping the second " and " was not closed, causing an error.

#2. How would you match the sequence "'\?

#3. What patterns will the regular expression \..\..\.. match? How would you represent it as a string?

```

```
## Error: <text>:5:25: unexpected symbol
## 4: str_view(x, "\") 
## 5: # escaping the second " and
##                            ^
```

