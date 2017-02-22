An Ode to the Pirate Language
================
Vivek Trivedi
16 February 2017

Summary:
========

### Vectors

-   By default, all R objects and most operations are vectors unless actively specified/manipulated.
-   Use square brackets to mess around with each value within the vector (Indexing).
-   Vectors with same flavour of objects (chr, num, logical etc.) are called atomic vectors. R will try to keep all vectors atomic by converting all values of the vector to the "lowest common denominator" flavour, generally character. Be careful, especially because this doesn't always throw up errors until it's too late (\#noragrets).
-   logical = TRUE and FALSE. numeric: numbers, integers and double-precision floating point numbers. character = ABCs and stuff.

``` r
x <- 3 * 4 # x <- 12

x[3] <- 9 # x <- c(12, NA, 9)

str(x) # num with 3 values
```

    ##  num [1:3] 12 NA 9

``` r
x[5]
```

    ## [1] NA

``` r
y <- x^pi

y
```

    ## [1] 2456.6760        NA  995.0416

``` r
# Same as this convoluted thing:
# 
# z <- vector(mode = mode(x), length = length(x))
# for(i in seq_along(x)) {
#   z[i] <- x[i]^2
# }
# 
# yay!

x[2] <- "bla" 

length(x)
```

    ## [1] 3

``` r
str(x)
```

    ##  chr [1:3] "12" "bla" "9"

``` r
z <- runif(5)

str(z)
```

    ##  num [1:5] 0.757 0.559 0.553 0.811 0.114

``` r
is.character(z) # FALSE
```

    ## [1] FALSE

``` r
names(z) <- letters[seq_along(z)] # Gives assigns smaller case letters 
# to each value in the specified vector.

z
```

    ##         a         b         c         d         e 
    ## 0.7573625 0.5585059 0.5534271 0.8109285 0.1135294

``` r
z < 0.5 # logical vector
```

    ##     a     b     c     d     e 
    ## FALSE FALSE FALSE FALSE  TRUE

``` r
as.numeric(z < 0.5) # FALSE = 0, TRUE = 1
```

    ## [1] 0 0 0 0 1

``` r
# is.bla() and as.bla() work for other flavours too (with varying success).

round(rnorm(5, mean = 10^(1:5))) # waaaattttt?!
```

    ## [1]     11     98   1001   9998 100000

**Note:** The funny business around the ability for R to take in vectors where intuitively there should not be vectors is something to be wary of.

------------------------------------------------------------------------

### Lists

-   If you want vectors that are non-atomic and don't devolve into the "lowest common denominator" flavour, you want lists. Dataframes are lists. Many functions return lists for ease of extraction and analysis.
-   Putting a set of smooth circular brackets around a function seems to display the result of the function.
-   Putting single square brackets around an index number provides the contents of the cell/s as a list. Putting double square brackets around them gives the actual contents of the cell/s. All the rest of the stuff about lists applies to data frames as well.

``` r
(a <- list("cabbage", pi, TRUE, 4.3))
```

    ## [[1]]
    ## [1] "cabbage"
    ## 
    ## [[2]]
    ## [1] 3.141593
    ## 
    ## [[3]]
    ## [1] TRUE
    ## 
    ## [[4]]
    ## [1] 4.3

``` r
names(a) <- c("veg", "irr_num", "Boolean", "number")

# OR OR OR...

b <- list(
  veg = "cabbage",
  irr_num = pi, 
  Boolean = TRUE, 
  number = 4.3)

identical(a,b) # TRUE.
```

    ## [1] TRUE

``` r
# Personally, I'd prefer b as I don't need to write double quotes as often.

length(a) # 4
```

    ## [1] 4

``` r
a$veg[2] = "broccolli" # both = or <- work, == doesn't

length(a) # still 4, not 5...
```

    ## [1] 4

``` r
class(a)
```

    ## [1] "list"

``` r
mode(a)
```

    ## [1] "list"

``` r
str(a)
```

    ## List of 4
    ##  $ veg    : chr [1:2] "cabbage" "broccolli"
    ##  $ irr_num: num 3.14
    ##  $ Boolean: logi TRUE
    ##  $ number : num 4.3

``` r
a[1] # type list
```

    ## $veg
    ## [1] "cabbage"   "broccolli"

``` r
a[[1]] # type character
```

    ## [1] "cabbage"   "broccolli"

``` r
# Group names can also be used to get the contents instead of the group index numbers. Same square bracket logic applies. 

a[["veg"]]
```

    ## [1] "cabbage"   "broccolli"

``` r
a[c("veg", "Boolean")] # works, returns a list of 2 lists! However, double square brackets don't work for more than one name/index.
```

    ## $veg
    ## [1] "cabbage"   "broccolli"
    ## 
    ## $Boolean
    ## [1] TRUE

``` r
length(a["veg"]) # 1, not 2...
```

    ## [1] 1

``` r
length(a["veg"][[1]]) # To get the qualities of the actual vector itself, rather than a list comprised of it.
```

    ## [1] 2

-   Character values under a column in data frames by default are converted to factor levels. To prevent this, use `I()`. aka Class 'AsIs'.

``` r
demodf <- data.frame(
  x = letters[1:3],
  y = I(letters[1:3]),
  z = 1:3 # Use equals sign for column value assignment. <- does something weird.
  )

str(demodf)
```

    ## 'data.frame':    3 obs. of  3 variables:
    ##  $ x: Factor w/ 3 levels "a","b","c": 1 2 3
    ##  $ y:Class 'AsIs'  chr [1:3] "a" "b" "c"
    ##  $ z: int  1 2 3

-   If each column/variables don't have the same langth, `as.data.frame()` won't work on lists. There are ways, however.

------------------------------------------------------------------------

### Matrices

-   Matrices are atomic vectors in two dimensions.
-   Indexing in matrices works very similarly to lists. Some interesting exceptions are given below.

``` r
matrix(LETTERS[1:25], nrow = 5) # Use byrow = TRUE argument for doing sequentially by the row.
```

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,] "A"  "F"  "K"  "P"  "U" 
    ## [2,] "B"  "G"  "L"  "Q"  "V" 
    ## [3,] "C"  "H"  "M"  "R"  "W" 
    ## [4,] "D"  "I"  "N"  "S"  "X" 
    ## [5,] "E"  "J"  "O"  "T"  "Y"

``` r
matrix("bla", nrow = 2, ncol = 5)
```

    ##      [,1]  [,2]  [,3]  [,4]  [,5] 
    ## [1,] "bla" "bla" "bla" "bla" "bla"
    ## [2,] "bla" "bla" "bla" "bla" "bla"

``` r
matrix(c("yo!", "foo?"), nrow = 3, ncol = 2) # Throws up soft error if total number of matrix elements is not a multiple of the total number of data values. Also, none of the two data elements are next to themselves in the matrix.
```

    ##      [,1]   [,2]  
    ## [1,] "yo!"  "foo?"
    ## [2,] "foo?" "yo!" 
    ## [3,] "yo!"  "foo?"

``` r
matrix(c("yo!", "foo?", "bla"), nrow = 6, ncol = 6) # If nrow is a multiple of the number of data elements, each row will have the same data element. Otherwise, as before.
```

    ##      [,1]   [,2]   [,3]   [,4]   [,5]   [,6]  
    ## [1,] "yo!"  "yo!"  "yo!"  "yo!"  "yo!"  "yo!" 
    ## [2,] "foo?" "foo?" "foo?" "foo?" "foo?" "foo?"
    ## [3,] "bla"  "bla"  "bla"  "bla"  "bla"  "bla" 
    ## [4,] "yo!"  "yo!"  "yo!"  "yo!"  "yo!"  "yo!" 
    ## [5,] "foo?" "foo?" "foo?" "foo?" "foo?" "foo?"
    ## [6,] "bla"  "bla"  "bla"  "bla"  "bla"  "bla"

``` r
vecDat <- data.frame(vec1 = 5:1,
                     vec2 = 2^(1:5))
vecMat <- as.matrix(vecDat)

str(vecMat) # Reminder: column names don't automatically turn into factors.
```

    ##  num [1:5, 1:2] 5 4 3 2 1 2 4 8 16 32
    ##  - attr(*, "dimnames")=List of 2
    ##   ..$ : NULL
    ##   ..$ : chr [1:2] "vec1" "vec2"

``` r
matrix(1:15, nrow = 5,
       dimnames = list(paste0("row", 1:5),
                       paste0("col", 1:3))) 
```

    ##      col1 col2 col3
    ## row1    1    6   11
    ## row2    2    7   12
    ## row3    3    8   13
    ## row4    4    9   14
    ## row5    5   10   15

``` r
#Use dimnames (with lists) for naming rows and columns in                                              that order.

# OR OR OR...

col1 <- row1 <- 1:5
col2 <- row2 <- 6:10
col3 <- row3 <- 11:15
cbind(col1, col2, col3)
```

    ##      col1 col2 col3
    ## [1,]    1    6   11
    ## [2,]    2    7   12
    ## [3,]    3    8   13
    ## [4,]    4    9   14
    ## [5,]    5   10   15

``` r
# OR...

Mtx <- rbind(row1, row2, row3)
colnames(Mtx) <- paste0("col", seq_len(ncol(Mtx))) # dimnames() can also be used.

Mtx
```

    ##      col1 col2 col3 col4 col5
    ## row1    1    2    3    4    5
    ## row2    6    7    8    9   10
    ## row3   11   12   13   14   15

``` r
Mtx[ , 3]
```

    ## row1 row2 row3 
    ##    3    8   13

``` r
Mtx[,3, drop = FALSE]
```

    ##      col3
    ## row1    3
    ## row2    8
    ## row3   13
