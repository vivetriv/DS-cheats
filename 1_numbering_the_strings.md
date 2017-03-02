Numbering the strings
================
Vivek Trivedi
26 February 2017

Summary
=======

### Not regex

-   Use `tidyverse` and `stringr` for messing with strings.
-   Massive topic, see the [Resources](http://stat545.com/block028_character-data.html#resources) if a problem occurs.
-   `fruit`, `words`, and `sentences` are prebuilt datasets within `stringr`.
-   `str_detect(vector, "str")` : Answers the condition of the object or an element of the object contains the given string, discretely by itself or within a larger element. Also works for regex.
-   `append(vector, new_element)` : Adds new elements to a vector.
-   `str_subset(vector, "str")` : Similar to `str_detect()`, except this command also produces a new factor with the elements of the given vectors that match the specified condition.

``` r
fruit # chr vector
```

    ##  [1] "apple"             "apricot"           "avocado"          
    ##  [4] "banana"            "bell pepper"       "bilberry"         
    ##  [7] "blackberry"        "blackcurrant"      "blood orange"     
    ## [10] "blueberry"         "boysenberry"       "breadfruit"       
    ## [13] "canary melon"      "cantaloupe"        "cherimoya"        
    ## [16] "cherry"            "chili pepper"      "clementine"       
    ## [19] "cloudberry"        "coconut"           "cranberry"        
    ## [22] "cucumber"          "currant"           "damson"           
    ## [25] "date"              "dragonfruit"       "durian"           
    ## [28] "eggplant"          "elderberry"        "feijoa"           
    ## [31] "fig"               "goji berry"        "gooseberry"       
    ## [34] "grape"             "grapefruit"        "guava"            
    ## [37] "honeydew"          "huckleberry"       "jackfruit"        
    ## [40] "jambul"            "jujube"            "kiwi fruit"       
    ## [43] "kumquat"           "lemon"             "lime"             
    ## [46] "loquat"            "lychee"            "mandarine"        
    ## [49] "mango"             "mulberry"          "nectarine"        
    ## [52] "nut"               "olive"             "orange"           
    ## [55] "pamelo"            "papaya"            "passionfruit"     
    ## [58] "peach"             "pear"              "persimmon"        
    ## [61] "physalis"          "pineapple"         "plum"             
    ## [64] "pomegranate"       "pomelo"            "purple mangosteen"
    ## [67] "quince"            "raisin"            "rambutan"         
    ## [70] "raspberry"         "redcurrant"        "rock melon"       
    ## [73] "salal berry"       "satsuma"           "star fruit"       
    ## [76] "strawberry"        "tamarillo"         "tangerine"        
    ## [79] "ugli fruit"        "watermelon"

``` r
index <- NULL
item <- NULL

for (n in 1:length(fruit)) {
  if (str_detect(fruit[n], "fruit") == TRUE) {
    index <- append(index, n)
    item <- append(item, fruit[n])
  }
}

(fru <- tibble(index, item))
```

    ## # A tibble: 8 × 2
    ##   index         item
    ##   <int>        <chr>
    ## 1    12   breadfruit
    ## 2    26  dragonfruit
    ## 3    35   grapefruit
    ## 4    39    jackfruit
    ## 5    42   kiwi fruit
    ## 6    57 passionfruit
    ## 7    75   star fruit
    ## 8    79   ugli fruit

``` r
# OR OR OR...

(fru2 <- str_subset(fruit, "fruit")) # It would be nice to have indices though.
```

    ## [1] "breadfruit"   "dragonfruit"  "grapefruit"   "jackfruit"   
    ## [5] "kiwi fruit"   "passionfruit" "star fruit"   "ugli fruit"

-   `str_split(vector, "delimiter")` : Outputs a list of elements of a vector split by the given delimiter. Often the delimiter is space, tab, comma, etc.
-   `str_split_fixed(vector, "delimiter" n = num)` : Outputs a length(vector) x num matrix of elements split by the given delimiter.
-   `separate(tbl_df, col, into = c("col1, col2"), sep = "bla")` : Same as `str_split()`, but works on data frames instead of vectors. Produces a tidy data frame with the given column names. **Note:** `into` argument needs to be supplied. It has no default.

``` r
str_split(fru2, " ") # class list
```

    ## [[1]]
    ## [1] "breadfruit"
    ## 
    ## [[2]]
    ## [1] "dragonfruit"
    ## 
    ## [[3]]
    ## [1] "grapefruit"
    ## 
    ## [[4]]
    ## [1] "jackfruit"
    ## 
    ## [[5]]
    ## [1] "kiwi"  "fruit"
    ## 
    ## [[6]]
    ## [1] "passionfruit"
    ## 
    ## [[7]]
    ## [1] "star"  "fruit"
    ## 
    ## [[8]]
    ## [1] "ugli"  "fruit"

``` r
str_split_fixed(fru2, " ", n = 3) # class matrix
```

    ##      [,1]           [,2]    [,3]
    ## [1,] "breadfruit"   ""      ""  
    ## [2,] "dragonfruit"  ""      ""  
    ## [3,] "grapefruit"   ""      ""  
    ## [4,] "jackfruit"    ""      ""  
    ## [5,] "kiwi"         "fruit" ""  
    ## [6,] "passionfruit" ""      ""  
    ## [7,] "star"         "fruit" ""  
    ## [8,] "ugli"         "fruit" ""

``` r
separate(fru, item, c("x" , "fruit"), " ") # class tbl_df
```

    ## Warning: Too few values at 5 locations: 1, 2, 3, 4, 6

    ## # A tibble: 8 × 3
    ##   index            x fruit
    ## * <int>        <chr> <chr>
    ## 1    12   breadfruit  <NA>
    ## 2    26  dragonfruit  <NA>
    ## 3    35   grapefruit  <NA>
    ## 4    39    jackfruit  <NA>
    ## 5    42         kiwi fruit
    ## 6    57 passionfruit  <NA>
    ## 7    75         star fruit
    ## 8    79         ugli fruit

-   `str_length(vector)` : Gives the length of strings within a vector.
-   `str_sub(vector, start_pos, end_pos)` : Output substrings including and in between the given character positions of the vector elements. Sliding windows and assignments are also possible (with caveats). Also see `str_replace()`.

``` r
str_length(fru2) # vector
```

    ## [1] 10 11 10  9 10 12 10 10

``` r
str_sub(fru2, 4, ) # vector
```

    ## [1] "adfruit"   "gonfruit"  "pefruit"   "kfruit"    "i fruit"   "sionfruit"
    ## [7] "r fruit"   "i fruit"

``` r
str_sub(fru2, 1:6, 3:8) # vector
```

    ## Warning in stri_sub(string, from = start, to = end): longer object length
    ## is not a multiple of shorter object length

    ## [1] "bre" "rag" "ape" "kfr" " fr" "onf" "sta" "gli"

``` r
fru3 <- fru2
mehfruit <- str_sub(fru3, 1, 4) <- "meh" 
mehfruit # Not how it works...
```

    ## [1] "meh"

``` r
# Only works this way...
str_sub(fru3, 1, 4) <- "meh"
fru3 # vector
```

    ## [1] "mehfruit"   "mehnfruit"  "mehfruit"   "mehruit"    "mehfruit"  
    ## [6] "mehonfruit" "mehfruit"   "mehfruit"

-   `str_c(vector_elements1, vector_elements2, sep = "bli", collapse = "bla")` : Useful little thing that can concantenate strings in a vector into larger strings. Opposite of `str_split()` and `separate()`.
-   `unite(tbl_df, "new_col", col1, col2..., sep = " or ")`: Combines two character-type tibble columns into a single column with a combiner string.

``` r
str_c(fru2[1:4], fru2[5:8], sep = " or may be ")
```

    ## [1] "breadfruit or may be kiwi fruit"   
    ## [2] "dragonfruit or may be passionfruit"
    ## [3] "grapefruit or may be star fruit"   
    ## [4] "jackfruit or may be ugli fruit"

``` r
str_c(fru2, collapse = ", ")
```

    ## [1] "breadfruit, dragonfruit, grapefruit, jackfruit, kiwi fruit, passionfruit, star fruit, ugli fruit"

``` r
str_c(fru2[1:4], fru2[5:8], sep = " or may be ", collapse = ", ")
```

    ## [1] "breadfruit or may be kiwi fruit, dragonfruit or may be passionfruit, grapefruit or may be star fruit, jackfruit or may be ugli fruit"

``` r
fruco <- tibble(fruit1 = fru$item[1:4], fruit2 = fru$item[5:8]) # unite() doesn't like col arguments with indices, therefore, simplifying column names is necessary.
fruco %>%
   unite("fruits", fruit1, fruit2, sep = " & then ")
```

    ## # A tibble: 4 × 1
    ##                            fruits
    ## *                           <chr>
    ## 1    breadfruit & then kiwi fruit
    ## 2 dragonfruit & then passionfruit
    ## 3    grapefruit & then star fruit
    ## 4     jackfruit & then ugli fruit

-   `str_replace(vector, "replacee", "replacer")` : Replaces a substring with another string. `str_replace_na()` variant exists.
-   `replace_na(tbl_df, replace = list(col = "replacer"))` : Replaces `NA` values in a tibble with the replacer string.

``` r
fru21 <- fru
str_replace(fru21$item, "uit", "actal")
```

    ## [1] "breadfractal"   "dragonfractal"  "grapefractal"   "jackfractal"   
    ## [5] "kiwi fractal"   "passionfractal" "star fractal"   "ugli fractal"

``` r
fru21$item[3] <- NA # NULL doesn't work for some reason...
fru21$item
```

    ## [1] "breadfruit"   "dragonfruit"  NA             "jackfruit"   
    ## [5] "kiwi fruit"   "passionfruit" "star fruit"   "ugli fruit"

``` r
str_replace_na(fru21$item, "It works") # returns vector.
```

    ## [1] "breadfruit"   "dragonfruit"  "It works"     "jackfruit"   
    ## [5] "kiwi fruit"   "passionfruit" "star fruit"   "ugli fruit"

``` r
replace_na(fru21, replace = list(item = "NADA")) # returns data frame.
```

    ## # A tibble: 8 × 2
    ##   index         item
    ##   <int>        <chr>
    ## 1    12   breadfruit
    ## 2    26  dragonfruit
    ## 3    35         NADA
    ## 4    39    jackfruit
    ## 5    42   kiwi fruit
    ## 6    57 passionfruit
    ## 7    75   star fruit
    ## 8    79   ugli fruit

------------------------------------------------------------------------

### Regex

-   It's like learning a new language no one even likes to speak or read...
-   When strings cannot be called upon using another fixed string (e.g.: `str_replace(fru, "fruit", "vege")`), regular expressions become useful, as they deal with character patterns within strings.
-   Regex go between quoation marks.
-   When using `\`, make sure to put an extra `\` to escape the stringification of stuff between quotation marks. Double square brackets around `:bla:` for the same reason.

| Regex       | Meaning                       |
|-------------|-------------------------------|
| `.`         | a single character            |
| `\n`        | new line                      |
| `^`         | beginning of string           |
| `$`         | end of string                 |
| `\b`        | word boundary                 |
| `\B`        | not word boundary             |
| `\d`        | single digit                  |
| `/s`        | space aka `[:space:]`         |
| `[bla]`     | character class               |
| `[^bla]`    | not bla chr class             |
| `[:punct:]` | Punctuation                   |
| `*`         | 0 or more                     |
| `+`         | 1 or more                     |
| `?`         | 0 or 1                        |
| `{n}`       | Exactly n                     |
| `{n,}`      | at least n                    |
| `{,m}`      | Up to m                       |
| `{n,m}`     | Between and including n and m |
