# June7_Kazu
Kazu  
6/6/2017  


#11.2 Getting started

```r
# heights <- read_csv("data/heights.csv") #unable to read file
```
#11.2.1 Compared to base R
#11.2.2 Exercises

```r
#1. What function would you use to read a file where fields were separated with “|”?
?read_csv()
?read_fwf()
?fwf_widths();?fwf_positions()
?read_table()
?read_log()
?read_delim()
read_delim("1|2|3\n4|5|6", delim="|",col_names = c("x", "y", "z"))
```

```
## # A tibble: 2 × 3
##       x     y     z
##   <int> <int> <int>
## 1     1     2     3
## 2     4     5     6
```

```r
#2. Apart from file, skip, and comment, what other arguments do read_csv() and read_tsv() have in common?
?read_csv()
?read_tsv()
# comma separated values
# read_csv(file, col_names = TRUE, col_types = NULL,
#   locale = default_locale(), na = c("", "NA"), quoted_na = TRUE,
#   quote = "\"", comment = "", trim_ws = TRUE, skip = 0, n_max = Inf,
#   guess_max = min(1000, n_max), progress = show_progress())
# # tab separated values
# read_tsv(file, col_names = TRUE, col_types = NULL,
#   locale = default_locale(), na = c("", "NA"), quoted_na = TRUE,
#   quote = "\"", comment = "", trim_ws = TRUE, skip = 0, n_max = Inf,
#   guess_max = min(1000, n_max), progress = show_progress())

#3. What are the most important arguments to read_fwf()?
?read_fwf()
# I don't know.

#4. Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like " or '. By convention, read_csv() assumes that the quoting character will be ", and if you want to change it you’ll need to use read_delim() instead. What arguments do you need to specify to read the following text into a data frame?

 read_delim("x,y\n1,'a,b'",quote="''",delim=",",col_names=c("Y","Z"))
```

```
## # A tibble: 2 × 2
##       Y     Z
##   <chr> <chr>
## 1     x     y
## 2     1   a,b
```

```r
#5. Identify what is wrong with each of the following inline CSV files. What happens when you run the code?
read_csv("a,b\n1,2\n3,4\n5,6") # 
```

```
## # A tibble: 3 × 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     3     4
## 3     5     6
```

```r
read_csv("a,b\n1,2,3\n4,5,6") # \n is wrong position (different col numbers in each row)
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 2 columns 3 columns literal data
##   2  -- 2 columns 3 columns literal data
```

```
## # A tibble: 2 × 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

```r
read_csv("a,b,c\n1,2\n1,2,3,4") # the third col is missing in the first row (or intentional?), an extra column in the seoond raw
```

```
## Warning: 2 parsing failures.
## row col  expected    actual         file
##   1  -- 3 columns 2 columns literal data
##   2  -- 3 columns 4 columns literal data
```

```
## # A tibble: 2 × 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

```r
#read_csv("a,b\n\"1") #  # should be 
read_csv("a,b\n1,2") #  ? extra double quote ("), 2nd col in 2nd row is missing.
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <int>
## 1     1     2
```

```r
read_csv("a,b\n1,2\na,b") # It looks OK to me.
```

```
## # A tibble: 2 × 2
##       a     b
##   <chr> <chr>
## 1     1     2
## 2     a     b
```

```r
#read_csv("a;b\n1;3") # should be
read_delim("a;b\n1;3",delim=";")
```

```
## # A tibble: 1 × 2
##       a     b
##   <int> <int>
## 1     1     3
```
# 11.3 Parsing a vector

```r
str(parse_logical(c("TRUE", "FALSE", "NA")))
```

```
##  logi [1:3] TRUE FALSE NA
```

```r
#>  logi [1:3] TRUE FALSE NA
str(parse_integer(c("1", "2", "3")))
```

```
##  int [1:3] 1 2 3
```

```r
#>  int [1:3] 1 2 3
str(parse_date(c("2010-01-01", "1979-10-14")))
```

```
##  Date[1:2], format: "2010-01-01" "1979-10-14"
```

```r
#>  Date[1:2], format: "2010-01-01" "1979-10-14"
parse_integer(c("1", "231", ".", "456"), na = ".")
```

```
## [1]   1 231  NA 456
```

```r
x <- parse_integer(c("123", "345", "abc", "123.45"))
```

```
## Warning: 2 parsing failures.
## row col               expected actual
##   3  -- an integer                abc
##   4  -- no trailing characters    .45
```

```r
problems(x)
```

```
## # A tibble: 2 × 4
##     row   col               expected actual
##   <int> <int>                  <chr>  <chr>
## 1     3    NA             an integer    abc
## 2     4    NA no trailing characters    .45
```
# 11.3.1 Numbers

```r
parse_double("1.23")
```

```
## [1] 1.23
```

```r
parse_double("1,23", locale = locale(decimal_mark = ","))
```

```
## [1] 1.23
```

```r
parse_number("$100")
```

```
## [1] 100
```

```r
parse_number("20%")
```

```
## [1] 20
```

```r
parse_number("It cost $123.45")
```

```
## [1] 123.45
```

```r
# Used in America
parse_number("$123,456,789")
```

```
## [1] 123456789
```

```r
# Used in many parts of Europe
parse_number("123.456.789", locale = locale(grouping_mark = "."))
```

```
## [1] 123456789
```

```r
# Used in Switzerland
parse_number("123'456'789", locale = locale(grouping_mark = "'"))
```

```
## [1] 123456789
```
# 11.3.2 Strings

```r
charToRaw("Hadley")
```

```
## [1] 48 61 64 6c 65 79
```

```r
x1 <- "El Ni\xf1o was particularly bad this year"
x2 <- "\x82\xb1\x82\xf1\x82\xc9\x82\xbf\x82\xcd"
x1
```

```
## [1] "El Ni\xf1o was particularly bad this year"
```

```r
x2
```

```
## [1] "\x82\xb1\x82\xf1\x82\u0242\xbf\x82\xcd"
```

```r
parse_character(x1, locale = locale(encoding = "Latin1"))
```

```
## [1] "El Niño was particularly bad this year"
```

```r
parse_character(x2, locale = locale(encoding = "Shift-JIS"))
```

```
## [1] "こんにちは"
```

```r
# how to find encoding? Useful.
guess_encoding(charToRaw(x1))
```

```
## # A tibble: 2 × 2
##     encoding confidence
##        <chr>      <dbl>
## 1 ISO-8859-1       0.46
## 2 ISO-8859-9       0.23
```

```r
guess_encoding(charToRaw(x2))
```

```
## # A tibble: 1 × 2
##   encoding confidence
##      <chr>      <dbl>
## 1   KOI8-R       0.42
```
# 11.3.3 Factors

```r
fruit <- c("apple", "banana")
parse_factor(c("apple", "banana", "bananana"), levels = fruit)
```

```
## Warning: 1 parsing failure.
## row col           expected   actual
##   3  -- value in level set bananana
```

```
## [1] apple  banana <NA>  
## attr(,"problems")
## # A tibble: 1 × 4
##     row   col           expected   actual
##   <int> <int>              <chr>    <chr>
## 1     3    NA value in level set bananana
## Levels: apple banana
```
# 11.3.4 Dates, date-times, and times

```r
parse_datetime("2010-10-01T2010")
```

```
## [1] "2010-10-01 20:10:00 UTC"
```

```r
parse_datetime("20101010")
```

```
## [1] "2010-10-10 UTC"
```

```r
parse_date("2010-10-01")
```

```
## [1] "2010-10-01"
```

```r
library(hms)
parse_time("01:10 am")
```

```
## 01:10:00
```

```r
parse_time("20:10:01")
```

```
## 20:10:01
```

```r
parse_date("01/02/15", "%m/%d/%y")
```

```
## [1] "2015-01-02"
```

```r
parse_date("01/02/15", "%d/%m/%y")
```

```
## [1] "2015-02-01"
```

```r
parse_date("01/02/15", "%y/%m/%d")
```

```
## [1] "2001-02-15"
```

```r
parse_date("1 janvier 2015", "%d %B %Y", locale = locale("fr")) 
```

```
## [1] "2015-01-01"
```
# 11.3.5 Exercises

```r
#1. What are the most important arguments to locale()?
?locale() # tz?

#2. What happens if you try and set decimal_mark and grouping_mark to the same character? What happens to the default value of grouping_mark when you set decimal_mark to “,”? What happens to the default value of decimal_mark when you set the grouping_mark to “.”?
#parse_double("1,23", locale = locale(decimal_mark = ",",grouping_mark = ","))
# Error: `decimal_mark` and `grouping_mark` must be different
parse_double("1,23", locale = locale(decimal_mark = ","))
```

```
## [1] 1.23
```

```r
#parse_double("1,23", locale = locale(grouping_mark = ","))
```

#3. I didn’t discuss the date_format and time_format options to locale(). What do they do? Construct an example that shows when they might be useful.

```r
parse_datetime("201501250215", locale = locale(date_format="%AD",time_format="%AT")) # error
```

```
## Warning: 1 parsing failure.
## row col   expected       actual
##   1  -- date like  201501250215
```

```
## [1] NA
```

```r
time1.UTC<-parse_datetime("20150125 0215", locale = locale(date_format="%AD",time_format="%AT")) # does work
# time2<-parse_datetime("012520150300", locale =locale(date_format="%m%d%Y",time_format="%H%M")) # does not work
# time2<-parse_datetime("01252015 0300", locale =locale(date_format="%m%d%Y",time_format="%AT")) # does not work
# time2<-parse_datetime("01252015T0300", locale =locale(date_format="%m%d%Y",time_format="%H%M")) # does not work

parse_date("01252015", locale = locale(date_format="%m%d%Y")) # does work
```

```
## [1] "2015-01-25"
```

```r
# parse_datetime("01252015", locale = locale(date_format="%m%d%Y")) # error
# 
# parse_date("01252015", locale = locale(date_format="%AD")) # error
# parse_datetime("01252015 0315", locale = locale(date_format="%m%d%Y",time_format="%AT")) # error
# parse_datetime("01252015 0315", locale = locale(date_format="%AD",time_format="%AT")) # error
# parse_datetime("012520150315", locale = locale(date_format="%m%d%Y",time_format="%H%M")) # error

#4. If you live outside the US, create a new locale object that encapsulates the settings for the types of file you read most commonly.
OlsonNames() # for UTC see https://en.wikipedia.org/wiki/Coordinated_Universal_Time
```

```
##   [1] "Africa/Abidjan"                   "Africa/Accra"                    
##   [3] "Africa/Addis_Ababa"               "Africa/Algiers"                  
##   [5] "Africa/Asmara"                    "Africa/Asmera"                   
##   [7] "Africa/Bamako"                    "Africa/Bangui"                   
##   [9] "Africa/Banjul"                    "Africa/Bissau"                   
##  [11] "Africa/Blantyre"                  "Africa/Brazzaville"              
##  [13] "Africa/Bujumbura"                 "Africa/Cairo"                    
##  [15] "Africa/Casablanca"                "Africa/Ceuta"                    
##  [17] "Africa/Conakry"                   "Africa/Dakar"                    
##  [19] "Africa/Dar_es_Salaam"             "Africa/Djibouti"                 
##  [21] "Africa/Douala"                    "Africa/El_Aaiun"                 
##  [23] "Africa/Freetown"                  "Africa/Gaborone"                 
##  [25] "Africa/Harare"                    "Africa/Johannesburg"             
##  [27] "Africa/Juba"                      "Africa/Kampala"                  
##  [29] "Africa/Khartoum"                  "Africa/Kigali"                   
##  [31] "Africa/Kinshasa"                  "Africa/Lagos"                    
##  [33] "Africa/Libreville"                "Africa/Lome"                     
##  [35] "Africa/Luanda"                    "Africa/Lubumbashi"               
##  [37] "Africa/Lusaka"                    "Africa/Malabo"                   
##  [39] "Africa/Maputo"                    "Africa/Maseru"                   
##  [41] "Africa/Mbabane"                   "Africa/Mogadishu"                
##  [43] "Africa/Monrovia"                  "Africa/Nairobi"                  
##  [45] "Africa/Ndjamena"                  "Africa/Niamey"                   
##  [47] "Africa/Nouakchott"                "Africa/Ouagadougou"              
##  [49] "Africa/Porto-Novo"                "Africa/Sao_Tome"                 
##  [51] "Africa/Timbuktu"                  "Africa/Tripoli"                  
##  [53] "Africa/Tunis"                     "Africa/Windhoek"                 
##  [55] "America/Adak"                     "America/Anchorage"               
##  [57] "America/Anguilla"                 "America/Antigua"                 
##  [59] "America/Araguaina"                "America/Argentina/Buenos_Aires"  
##  [61] "America/Argentina/Catamarca"      "America/Argentina/ComodRivadavia"
##  [63] "America/Argentina/Cordoba"        "America/Argentina/Jujuy"         
##  [65] "America/Argentina/La_Rioja"       "America/Argentina/Mendoza"       
##  [67] "America/Argentina/Rio_Gallegos"   "America/Argentina/Salta"         
##  [69] "America/Argentina/San_Juan"       "America/Argentina/San_Luis"      
##  [71] "America/Argentina/Tucuman"        "America/Argentina/Ushuaia"       
##  [73] "America/Aruba"                    "America/Asuncion"                
##  [75] "America/Atikokan"                 "America/Atka"                    
##  [77] "America/Bahia"                    "America/Bahia_Banderas"          
##  [79] "America/Barbados"                 "America/Belem"                   
##  [81] "America/Belize"                   "America/Blanc-Sablon"            
##  [83] "America/Boa_Vista"                "America/Bogota"                  
##  [85] "America/Boise"                    "America/Buenos_Aires"            
##  [87] "America/Cambridge_Bay"            "America/Campo_Grande"            
##  [89] "America/Cancun"                   "America/Caracas"                 
##  [91] "America/Catamarca"                "America/Cayenne"                 
##  [93] "America/Cayman"                   "America/Chicago"                 
##  [95] "America/Chihuahua"                "America/Coral_Harbour"           
##  [97] "America/Cordoba"                  "America/Costa_Rica"              
##  [99] "America/Creston"                  "America/Cuiaba"                  
## [101] "America/Curacao"                  "America/Danmarkshavn"            
## [103] "America/Dawson"                   "America/Dawson_Creek"            
## [105] "America/Denver"                   "America/Detroit"                 
## [107] "America/Dominica"                 "America/Edmonton"                
## [109] "America/Eirunepe"                 "America/El_Salvador"             
## [111] "America/Ensenada"                 "America/Fort_Nelson"             
## [113] "America/Fort_Wayne"               "America/Fortaleza"               
## [115] "America/Glace_Bay"                "America/Godthab"                 
## [117] "America/Goose_Bay"                "America/Grand_Turk"              
## [119] "America/Grenada"                  "America/Guadeloupe"              
## [121] "America/Guatemala"                "America/Guayaquil"               
## [123] "America/Guyana"                   "America/Halifax"                 
## [125] "America/Havana"                   "America/Hermosillo"              
## [127] "America/Indiana/Indianapolis"     "America/Indiana/Knox"            
## [129] "America/Indiana/Marengo"          "America/Indiana/Petersburg"      
## [131] "America/Indiana/Tell_City"        "America/Indiana/Vevay"           
## [133] "America/Indiana/Vincennes"        "America/Indiana/Winamac"         
## [135] "America/Indianapolis"             "America/Inuvik"                  
## [137] "America/Iqaluit"                  "America/Jamaica"                 
## [139] "America/Jujuy"                    "America/Juneau"                  
## [141] "America/Kentucky/Louisville"      "America/Kentucky/Monticello"     
## [143] "America/Knox_IN"                  "America/Kralendijk"              
## [145] "America/La_Paz"                   "America/Lima"                    
## [147] "America/Los_Angeles"              "America/Louisville"              
## [149] "America/Lower_Princes"            "America/Maceio"                  
## [151] "America/Managua"                  "America/Manaus"                  
## [153] "America/Marigot"                  "America/Martinique"              
## [155] "America/Matamoros"                "America/Mazatlan"                
## [157] "America/Mendoza"                  "America/Menominee"               
## [159] "America/Merida"                   "America/Metlakatla"              
## [161] "America/Mexico_City"              "America/Miquelon"                
## [163] "America/Moncton"                  "America/Monterrey"               
## [165] "America/Montevideo"               "America/Montreal"                
## [167] "America/Montserrat"               "America/Nassau"                  
## [169] "America/New_York"                 "America/Nipigon"                 
## [171] "America/Nome"                     "America/Noronha"                 
## [173] "America/North_Dakota/Beulah"      "America/North_Dakota/Center"     
## [175] "America/North_Dakota/New_Salem"   "America/Ojinaga"                 
## [177] "America/Panama"                   "America/Pangnirtung"             
## [179] "America/Paramaribo"               "America/Phoenix"                 
## [181] "America/Port_of_Spain"            "America/Port-au-Prince"          
## [183] "America/Porto_Acre"               "America/Porto_Velho"             
## [185] "America/Puerto_Rico"              "America/Rainy_River"             
## [187] "America/Rankin_Inlet"             "America/Recife"                  
## [189] "America/Regina"                   "America/Resolute"                
## [191] "America/Rio_Branco"               "America/Rosario"                 
## [193] "America/Santa_Isabel"             "America/Santarem"                
## [195] "America/Santiago"                 "America/Santo_Domingo"           
## [197] "America/Sao_Paulo"                "America/Scoresbysund"            
## [199] "America/Shiprock"                 "America/Sitka"                   
## [201] "America/St_Barthelemy"            "America/St_Johns"                
## [203] "America/St_Kitts"                 "America/St_Lucia"                
## [205] "America/St_Thomas"                "America/St_Vincent"              
## [207] "America/Swift_Current"            "America/Tegucigalpa"             
## [209] "America/Thule"                    "America/Thunder_Bay"             
## [211] "America/Tijuana"                  "America/Toronto"                 
## [213] "America/Tortola"                  "America/Vancouver"               
## [215] "America/Virgin"                   "America/Whitehorse"              
## [217] "America/Winnipeg"                 "America/Yakutat"                 
## [219] "America/Yellowknife"              "Antarctica/Casey"                
## [221] "Antarctica/Davis"                 "Antarctica/DumontDUrville"       
## [223] "Antarctica/Macquarie"             "Antarctica/Mawson"               
## [225] "Antarctica/McMurdo"               "Antarctica/Palmer"               
## [227] "Antarctica/Rothera"               "Antarctica/South_Pole"           
## [229] "Antarctica/Syowa"                 "Antarctica/Troll"                
## [231] "Antarctica/Vostok"                "Arctic/Longyearbyen"             
## [233] "Asia/Aden"                        "Asia/Almaty"                     
## [235] "Asia/Amman"                       "Asia/Anadyr"                     
## [237] "Asia/Aqtau"                       "Asia/Aqtobe"                     
## [239] "Asia/Ashgabat"                    "Asia/Ashkhabad"                  
## [241] "Asia/Baghdad"                     "Asia/Bahrain"                    
## [243] "Asia/Baku"                        "Asia/Bangkok"                    
## [245] "Asia/Barnaul"                     "Asia/Beirut"                     
## [247] "Asia/Bishkek"                     "Asia/Brunei"                     
## [249] "Asia/Calcutta"                    "Asia/Chita"                      
## [251] "Asia/Choibalsan"                  "Asia/Chongqing"                  
## [253] "Asia/Chungking"                   "Asia/Colombo"                    
## [255] "Asia/Dacca"                       "Asia/Damascus"                   
## [257] "Asia/Dhaka"                       "Asia/Dili"                       
## [259] "Asia/Dubai"                       "Asia/Dushanbe"                   
## [261] "Asia/Gaza"                        "Asia/Harbin"                     
## [263] "Asia/Hebron"                      "Asia/Ho_Chi_Minh"                
## [265] "Asia/Hong_Kong"                   "Asia/Hovd"                       
## [267] "Asia/Irkutsk"                     "Asia/Istanbul"                   
## [269] "Asia/Jakarta"                     "Asia/Jayapura"                   
## [271] "Asia/Jerusalem"                   "Asia/Kabul"                      
## [273] "Asia/Kamchatka"                   "Asia/Karachi"                    
## [275] "Asia/Kashgar"                     "Asia/Kathmandu"                  
## [277] "Asia/Katmandu"                    "Asia/Khandyga"                   
## [279] "Asia/Kolkata"                     "Asia/Krasnoyarsk"                
## [281] "Asia/Kuala_Lumpur"                "Asia/Kuching"                    
## [283] "Asia/Kuwait"                      "Asia/Macao"                      
## [285] "Asia/Macau"                       "Asia/Magadan"                    
## [287] "Asia/Makassar"                    "Asia/Manila"                     
## [289] "Asia/Muscat"                      "Asia/Nicosia"                    
## [291] "Asia/Novokuznetsk"                "Asia/Novosibirsk"                
## [293] "Asia/Omsk"                        "Asia/Oral"                       
## [295] "Asia/Phnom_Penh"                  "Asia/Pontianak"                  
## [297] "Asia/Pyongyang"                   "Asia/Qatar"                      
## [299] "Asia/Qyzylorda"                   "Asia/Rangoon"                    
## [301] "Asia/Riyadh"                      "Asia/Saigon"                     
## [303] "Asia/Sakhalin"                    "Asia/Samarkand"                  
## [305] "Asia/Seoul"                       "Asia/Shanghai"                   
## [307] "Asia/Singapore"                   "Asia/Srednekolymsk"              
## [309] "Asia/Taipei"                      "Asia/Tashkent"                   
## [311] "Asia/Tbilisi"                     "Asia/Tehran"                     
## [313] "Asia/Tel_Aviv"                    "Asia/Thimbu"                     
## [315] "Asia/Thimphu"                     "Asia/Tokyo"                      
## [317] "Asia/Tomsk"                       "Asia/Ujung_Pandang"              
## [319] "Asia/Ulaanbaatar"                 "Asia/Ulan_Bator"                 
## [321] "Asia/Urumqi"                      "Asia/Ust-Nera"                   
## [323] "Asia/Vientiane"                   "Asia/Vladivostok"                
## [325] "Asia/Yakutsk"                     "Asia/Yekaterinburg"              
## [327] "Asia/Yerevan"                     "Atlantic/Azores"                 
## [329] "Atlantic/Bermuda"                 "Atlantic/Canary"                 
## [331] "Atlantic/Cape_Verde"              "Atlantic/Faeroe"                 
## [333] "Atlantic/Faroe"                   "Atlantic/Jan_Mayen"              
## [335] "Atlantic/Madeira"                 "Atlantic/Reykjavik"              
## [337] "Atlantic/South_Georgia"           "Atlantic/St_Helena"              
## [339] "Atlantic/Stanley"                 "Australia/ACT"                   
## [341] "Australia/Adelaide"               "Australia/Brisbane"              
## [343] "Australia/Broken_Hill"            "Australia/Canberra"              
## [345] "Australia/Currie"                 "Australia/Darwin"                
## [347] "Australia/Eucla"                  "Australia/Hobart"                
## [349] "Australia/LHI"                    "Australia/Lindeman"              
## [351] "Australia/Lord_Howe"              "Australia/Melbourne"             
## [353] "Australia/North"                  "Australia/NSW"                   
## [355] "Australia/Perth"                  "Australia/Queensland"            
## [357] "Australia/South"                  "Australia/Sydney"                
## [359] "Australia/Tasmania"               "Australia/Victoria"              
## [361] "Australia/West"                   "Australia/Yancowinna"            
## [363] "Brazil/Acre"                      "Brazil/DeNoronha"                
## [365] "Brazil/East"                      "Brazil/West"                     
## [367] "Canada/Atlantic"                  "Canada/Central"                  
## [369] "Canada/East-Saskatchewan"         "Canada/Eastern"                  
## [371] "Canada/Mountain"                  "Canada/Newfoundland"             
## [373] "Canada/Pacific"                   "Canada/Saskatchewan"             
## [375] "Canada/Yukon"                     "CET"                             
## [377] "Chile/Continental"                "Chile/EasterIsland"              
## [379] "CST6CDT"                          "Cuba"                            
## [381] "EET"                              "Egypt"                           
## [383] "Eire"                             "EST"                             
## [385] "EST5EDT"                          "Etc/GMT"                         
## [387] "Etc/GMT-0"                        "Etc/GMT-1"                       
## [389] "Etc/GMT-10"                       "Etc/GMT-11"                      
## [391] "Etc/GMT-12"                       "Etc/GMT-13"                      
## [393] "Etc/GMT-14"                       "Etc/GMT-2"                       
## [395] "Etc/GMT-3"                        "Etc/GMT-4"                       
## [397] "Etc/GMT-5"                        "Etc/GMT-6"                       
## [399] "Etc/GMT-7"                        "Etc/GMT-8"                       
## [401] "Etc/GMT-9"                        "Etc/GMT+0"                       
## [403] "Etc/GMT+1"                        "Etc/GMT+10"                      
## [405] "Etc/GMT+11"                       "Etc/GMT+12"                      
## [407] "Etc/GMT+2"                        "Etc/GMT+3"                       
## [409] "Etc/GMT+4"                        "Etc/GMT+5"                       
## [411] "Etc/GMT+6"                        "Etc/GMT+7"                       
## [413] "Etc/GMT+8"                        "Etc/GMT+9"                       
## [415] "Etc/GMT0"                         "Etc/Greenwich"                   
## [417] "Etc/UCT"                          "Etc/Universal"                   
## [419] "Etc/UTC"                          "Etc/Zulu"                        
## [421] "Europe/Amsterdam"                 "Europe/Andorra"                  
## [423] "Europe/Astrakhan"                 "Europe/Athens"                   
## [425] "Europe/Belfast"                   "Europe/Belgrade"                 
## [427] "Europe/Berlin"                    "Europe/Bratislava"               
## [429] "Europe/Brussels"                  "Europe/Bucharest"                
## [431] "Europe/Budapest"                  "Europe/Busingen"                 
## [433] "Europe/Chisinau"                  "Europe/Copenhagen"               
## [435] "Europe/Dublin"                    "Europe/Gibraltar"                
## [437] "Europe/Guernsey"                  "Europe/Helsinki"                 
## [439] "Europe/Isle_of_Man"               "Europe/Istanbul"                 
## [441] "Europe/Jersey"                    "Europe/Kaliningrad"              
## [443] "Europe/Kiev"                      "Europe/Kirov"                    
## [445] "Europe/Lisbon"                    "Europe/Ljubljana"                
## [447] "Europe/London"                    "Europe/Luxembourg"               
## [449] "Europe/Madrid"                    "Europe/Malta"                    
## [451] "Europe/Mariehamn"                 "Europe/Minsk"                    
## [453] "Europe/Monaco"                    "Europe/Moscow"                   
## [455] "Europe/Nicosia"                   "Europe/Oslo"                     
## [457] "Europe/Paris"                     "Europe/Podgorica"                
## [459] "Europe/Prague"                    "Europe/Riga"                     
## [461] "Europe/Rome"                      "Europe/Samara"                   
## [463] "Europe/San_Marino"                "Europe/Sarajevo"                 
## [465] "Europe/Simferopol"                "Europe/Skopje"                   
## [467] "Europe/Sofia"                     "Europe/Stockholm"                
## [469] "Europe/Tallinn"                   "Europe/Tirane"                   
## [471] "Europe/Tiraspol"                  "Europe/Ulyanovsk"                
## [473] "Europe/Uzhgorod"                  "Europe/Vaduz"                    
## [475] "Europe/Vatican"                   "Europe/Vienna"                   
## [477] "Europe/Vilnius"                   "Europe/Volgograd"                
## [479] "Europe/Warsaw"                    "Europe/Zagreb"                   
## [481] "Europe/Zaporozhye"                "Europe/Zurich"                   
## [483] "GB"                               "GB-Eire"                         
## [485] "GMT"                              "GMT-0"                           
## [487] "GMT+0"                            "GMT0"                            
## [489] "Greenwich"                        "Hongkong"                        
## [491] "HST"                              "Iceland"                         
## [493] "Indian/Antananarivo"              "Indian/Chagos"                   
## [495] "Indian/Christmas"                 "Indian/Cocos"                    
## [497] "Indian/Comoro"                    "Indian/Kerguelen"                
## [499] "Indian/Mahe"                      "Indian/Maldives"                 
## [501] "Indian/Mauritius"                 "Indian/Mayotte"                  
## [503] "Indian/Reunion"                   "Iran"                            
## [505] "Israel"                           "Jamaica"                         
## [507] "Japan"                            "Kwajalein"                       
## [509] "Libya"                            "MET"                             
## [511] "Mexico/BajaNorte"                 "Mexico/BajaSur"                  
## [513] "Mexico/General"                   "MST"                             
## [515] "MST7MDT"                          "Navajo"                          
## [517] "NZ"                               "NZ-CHAT"                         
## [519] "Pacific/Apia"                     "Pacific/Auckland"                
## [521] "Pacific/Bougainville"             "Pacific/Chatham"                 
## [523] "Pacific/Chuuk"                    "Pacific/Easter"                  
## [525] "Pacific/Efate"                    "Pacific/Enderbury"               
## [527] "Pacific/Fakaofo"                  "Pacific/Fiji"                    
## [529] "Pacific/Funafuti"                 "Pacific/Galapagos"               
## [531] "Pacific/Gambier"                  "Pacific/Guadalcanal"             
## [533] "Pacific/Guam"                     "Pacific/Honolulu"                
## [535] "Pacific/Johnston"                 "Pacific/Kiritimati"              
## [537] "Pacific/Kosrae"                   "Pacific/Kwajalein"               
## [539] "Pacific/Majuro"                   "Pacific/Marquesas"               
## [541] "Pacific/Midway"                   "Pacific/Nauru"                   
## [543] "Pacific/Niue"                     "Pacific/Norfolk"                 
## [545] "Pacific/Noumea"                   "Pacific/Pago_Pago"               
## [547] "Pacific/Palau"                    "Pacific/Pitcairn"                
## [549] "Pacific/Pohnpei"                  "Pacific/Ponape"                  
## [551] "Pacific/Port_Moresby"             "Pacific/Rarotonga"               
## [553] "Pacific/Saipan"                   "Pacific/Samoa"                   
## [555] "Pacific/Tahiti"                   "Pacific/Tarawa"                  
## [557] "Pacific/Tongatapu"                "Pacific/Truk"                    
## [559] "Pacific/Wake"                     "Pacific/Wallis"                  
## [561] "Pacific/Yap"                      "Poland"                          
## [563] "Portugal"                         "PRC"                             
## [565] "PST8PDT"                          "ROC"                             
## [567] "ROK"                              "Singapore"                       
## [569] "Turkey"                           "UCT"                             
## [571] "Universal"                        "US/Alaska"                       
## [573] "US/Aleutian"                      "US/Arizona"                      
## [575] "US/Central"                       "US/East-Indiana"                 
## [577] "US/Eastern"                       "US/Hawaii"                       
## [579] "US/Indiana-Starke"                "US/Michigan"                     
## [581] "US/Mountain"                      "US/Pacific"                      
## [583] "US/Pacific-New"                   "US/Samoa"                        
## [585] "UTC"                              "VERSION"                         
## [587] "W-SU"                             "WET"                             
## [589] "Zulu"
```

```r
time1.jp<-parse_datetime("20150125 0215", locale = locale(date_format="%AD",time_format="%AT",tz="Japan")) # does work
time1.UTC - time1.jp # Time difference of 9 hours
```

```
## Time difference of 9 hours
```

```r
#5. What’s the difference between read_csv() and read_csv2()?
?read.csv
?read.csv2
# read.csv(file, header = TRUE, sep = ",", quote = "\"",
#          dec = ".", fill = TRUE, comment.char = "", ...)
# 
# read.csv2(file, header = TRUE, sep = ";", quote = "\"",
#           dec = ",", fill = TRUE, comment.char = "", ...)
# dec: the character used in the file for decimal points

#6. What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.

# https://support.sas.com/documentation/cdl/en/nlsref/69741/HTML/default/viewer.htm#n0882t2muy4l94n19cno6z40xmuz.htm

#7. Generate the correct format string to parse each of the following dates and times:

d1 <- "January 1, 2010";parse_date(d1,"%B %d, %Y")
```

```
## [1] "2010-01-01"
```

```r
d2 <- "2015-Mar-07";parse_date(d2,"%Y-%b-%d")
```

```
## [1] "2015-03-07"
```

```r
d3 <- "06-Jun-2017";parse_date(d3,"%d-%b-%Y")
```

```
## [1] "2017-06-06"
```

```r
d4 <- c("August 19 (2015)", "July 1 (2015)");parse_date(d4,"%B %d (%Y)")
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
d5 <- "12/30/14" # Dec 30, 2014
parse_date(d5,"%m/%d/%y")
```

```
## [1] "2014-12-30"
```

```r
t1 <- "1705";parse_time(t1,"%H%M")
```

```
## 17:05:00
```

```r
t2 <- "11:15:10.12 PM";parse_time(t2,"%I:%M:%OS %p")
```

```
## 23:15:10.12
```
# 11.4 Parsing a file
# 11.4.1 Strategy

```r
guess_parser("2010-10-01") #> [1] "date"
```

```
## [1] "date"
```

```r
guess_parser("15:01") #> [1] "time"
```

```
## [1] "time"
```

```r
guess_parser(c("TRUE", "FALSE")) #> [1] "logical"
```

```
## [1] "logical"
```

```r
guess_parser(c("1", "5", "9")) #> [1] "integer"
```

```
## [1] "integer"
```

```r
guess_parser(c("12,352,561")) #> [1] "number"
```

```
## [1] "number"
```

```r
str(parse_guess("2010-10-10")) #>  Date[1:1], format: "2010-10-10"
```

```
##  Date[1:1], format: "2010-10-10"
```
# 11.4.2 Problems

```r
challenge <- read_csv(readr_example("challenge.csv"))
```

```
## Parsed with column specification:
## cols(
##   x = col_integer(),
##   y = col_character()
## )
```

```
## Warning: 1000 parsing failures.
##  row col               expected             actual                                                                                         file
## 1001   x no trailing characters .23837975086644292 '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1002   x no trailing characters .41167997173033655 '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1003   x no trailing characters .7460716762579978  '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1004   x no trailing characters .723450553836301   '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1005   x no trailing characters .614524137461558   '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## .... ... ...................... .................. ............................................................................................
## See problems(...) for more details.
```

```r
problems(challenge)
```

```
## # A tibble: 1,000 × 5
##      row   col               expected             actual
##    <int> <chr>                  <chr>              <chr>
## 1   1001     x no trailing characters .23837975086644292
## 2   1002     x no trailing characters .41167997173033655
## 3   1003     x no trailing characters  .7460716762579978
## 4   1004     x no trailing characters   .723450553836301
## 5   1005     x no trailing characters   .614524137461558
## 6   1006     x no trailing characters   .473980569280684
## 7   1007     x no trailing characters  .5784610391128808
## 8   1008     x no trailing characters  .2415937229525298
## 9   1009     x no trailing characters .11437866208143532
## 10  1010     x no trailing characters  .2983446326106787
## # ... with 990 more rows, and 1 more variables: file <chr>
```

```r
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_integer(),
    y = col_character()
  )
)
```

```
## Warning: 1000 parsing failures.
##  row col               expected             actual                                                                                         file
## 1001   x no trailing characters .23837975086644292 '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1002   x no trailing characters .41167997173033655 '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1003   x no trailing characters .7460716762579978  '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1004   x no trailing characters .723450553836301   '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## 1005   x no trailing characters .614524137461558   '/Library/Frameworks/R.framework/Versions/3.3/Resources/library/readr/extdata/challenge.csv'
## .... ... ...................... .................. ............................................................................................
## See problems(...) for more details.
```

```r
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_double(), # changed
    y = col_character()
  )
)
tail(challenge)
```

```
## # A tibble: 6 × 2
##           x          y
##       <dbl>      <chr>
## 1 0.8052743 2019-11-21
## 2 0.1635163 2018-03-29
## 3 0.4719390 2014-08-04
## 4 0.7183186 2015-08-16
## 5 0.2698786 2020-02-04
## 6 0.6082372 2019-01-06
```

```r
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_double(),
    y = col_date() # changed
  )
)
tail(challenge)
```

```
## # A tibble: 6 × 2
##           x          y
##       <dbl>     <date>
## 1 0.8052743 2019-11-21
## 2 0.1635163 2018-03-29
## 3 0.4719390 2014-08-04
## 4 0.7183186 2015-08-16
## 5 0.2698786 2020-02-04
## 6 0.6082372 2019-01-06
```
# 11.4.3 Other strategies

```r
challenge2 <- read_csv(readr_example("challenge.csv"), guess_max = 1001)
```

```
## Parsed with column specification:
## cols(
##   x = col_double(),
##   y = col_date(format = "")
## )
```

```r
challenge2
```

```
## # A tibble: 2,000 × 2
##        x      y
##    <dbl> <date>
## 1    404   <NA>
## 2   4172   <NA>
## 3   3004   <NA>
## 4    787   <NA>
## 5     37   <NA>
## 6   2332   <NA>
## 7   2489   <NA>
## 8   1449   <NA>
## 9   3665   <NA>
## 10  3863   <NA>
## # ... with 1,990 more rows
```

```r
challenge2 <- read_csv(readr_example("challenge.csv"), 
  col_types = cols(.default = col_character())
)
df <- tribble(
  ~x,  ~y,
  "1", "1.21",
  "2", "2.32",
  "3", "4.56"
)
df
```

```
## # A tibble: 3 × 2
##       x     y
##   <chr> <chr>
## 1     1  1.21
## 2     2  2.32
## 3     3  4.56
```

```r
type_convert(df)
```

```
## Parsed with column specification:
## cols(
##   x = col_integer(),
##   y = col_double()
## )
```

```
## # A tibble: 3 × 2
##       x     y
##   <int> <dbl>
## 1     1  1.21
## 2     2  2.32
## 3     3  4.56
```

```r
df
```

```
## # A tibble: 3 × 2
##       x     y
##   <chr> <chr>
## 1     1  1.21
## 2     2  2.32
## 3     3  4.56
```
# 11.5 Writing to a file

```r
write_csv(challenge, "challenge.csv")
challenge
```

```
## # A tibble: 2,000 × 2
##        x      y
##    <dbl> <date>
## 1    404   <NA>
## 2   4172   <NA>
## 3   3004   <NA>
## 4    787   <NA>
## 5     37   <NA>
## 6   2332   <NA>
## 7   2489   <NA>
## 8   1449   <NA>
## 9   3665   <NA>
## 10  3863   <NA>
## # ... with 1,990 more rows
```

```r
#Note that the type information is lost when you save to csv:
write_csv(challenge, "challenge-2.csv")
read_csv("challenge-2.csv")
```

```
## Parsed with column specification:
## cols(
##   x = col_integer(),
##   y = col_character()
## )
```

```
## Warning: 1000 parsing failures.
##  row col               expected             actual              file
## 1001   x no trailing characters .23837975086644292 'challenge-2.csv'
## 1002   x no trailing characters .41167997173033655 'challenge-2.csv'
## 1003   x no trailing characters .7460716762579978  'challenge-2.csv'
## 1004   x no trailing characters .723450553836301   'challenge-2.csv'
## 1005   x no trailing characters .614524137461558   'challenge-2.csv'
## .... ... ...................... .................. .................
## See problems(...) for more details.
```

```
## # A tibble: 2,000 × 2
##        x     y
##    <int> <chr>
## 1    404  <NA>
## 2   4172  <NA>
## 3   3004  <NA>
## 4    787  <NA>
## 5     37  <NA>
## 6   2332  <NA>
## 7   2489  <NA>
## 8   1449  <NA>
## 9   3665  <NA>
## 10  3863  <NA>
## # ... with 1,990 more rows
```

```r
# This makes CSVs a little unreliable for caching interim results—you need to recreate the column specification every time you load in. There are two alternatives:
#1. write_rds() and read_rds() are uniform wrappers around the base functions readRDS() and saveRDS(). These store data in R’s custom binary format called RDS:
write_rds(challenge, "challenge.rds")
read_rds("challenge.rds")
```

```
## # A tibble: 2,000 × 2
##        x      y
##    <dbl> <date>
## 1    404   <NA>
## 2   4172   <NA>
## 3   3004   <NA>
## 4    787   <NA>
## 5     37   <NA>
## 6   2332   <NA>
## 7   2489   <NA>
## 8   1449   <NA>
## 9   3665   <NA>
## 10  3863   <NA>
## # ... with 1,990 more rows
```

```r
#2 The feather package implements a fast binary file format that can be shared across programming languages:
library(feather)
write_feather(challenge, "challenge.feather")
read_feather("challenge.feather")
```

```
## # A tibble: 2,000 × 2
##        x      y
##    <dbl> <date>
## 1    404   <NA>
## 2   4172   <NA>
## 3   3004   <NA>
## 4    787   <NA>
## 5     37   <NA>
## 6   2332   <NA>
## 7   2489   <NA>
## 8   1449   <NA>
## 9   3665   <NA>
## 10  3863   <NA>
## # ... with 1,990 more rows
```
# 11.6 Other types of data

