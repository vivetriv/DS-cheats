An Ode to the Pirate Language
================
Vivek Trivedi
16 February 2017

Summary:
========

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

    ##  num [1:5] 0.2729 0.1171 0.7737 0.0369 0.489

``` r
is.character(z) # FALSE
```

    ## [1] FALSE

``` r
names(z) <- letters[seq_along(z)] # Gives assigns smaller case letters to each value in the specified vector.

z
```

    ##          a          b          c          d          e 
    ## 0.27286419 0.11708719 0.77368677 0.03694595 0.48895029

``` r
z < 0.5 # logical vector
```

    ##     a     b     c     d     e 
    ##  TRUE  TRUE FALSE  TRUE  TRUE

``` r
as.numeric(z < 0.5) # FALSE = 0, TRUE = 1
```

    ## [1] 1 1 0 1 1

``` r
# is.bla() and as.bla() work for other flavours too (with varying success).

round(rnorm(5, mean = 10^(1:5))) # waaaattttt?!
```

    ## [1]    11   100  1000 10000 99999

**Note:** The funny business around the ability for R to take in vectors where intuitively there should not be vectors is something to be wary of.

-   If you want vectors that are non-atomic and don't devolve into the "lowest common denominator" flavour, you want lists. Dataframes are lists. Many functions return lists for ease of extraction and analysis.
-   Putting a set of smooth circular brackets around a function seems to display the result of the function.
-   Putting single square brackets around an index number provides the contents of the cell/s as a list. Putting double square brackets around them gives the actual contents of the cell/s.

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
# Group names can also be used to get the contents instead of 
# the group index numbers. Same square bracket logic applies. 

a[["veg"]]
```

    ## [1] "cabbage"   "broccolli"

``` r
a[c("veg", "Boolean")] # works, returns a list of 2 lists! 
```

    ## $veg
    ## [1] "cabbage"   "broccolli"
    ## 
    ## $Boolean
    ## [1] TRUE

``` r
# However, double square brackets don't work for more than one name/index.
```

-
